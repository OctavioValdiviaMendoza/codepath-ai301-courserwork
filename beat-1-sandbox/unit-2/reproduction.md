# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

OctavioValdiviaMendoza

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/35#issuecomment-5988938947

I’d like to investigate issue #35, which proposes adding a webhook endpoint where clients can register a callback URL and receive a POST containing the review payload after long-running, multi-repository reviews are complete.

I’ll review the current FastAPI review-processing flow and the relevant areas under `api/routes/` and `core/services/`, then test the review-completion path. I’ll share a report documenting my environment, steps, and observations before suggesting implementation work.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/35#issuecomment-5989801874

### Environment

Windows 11 Home 10.0.26200, Git Bash; Docker 28.5.1 / Compose v2.40.3; Postgres 16.15; Python 3.12.10; FastAPI 0.142.2, Uvicorn 0.54.0; upstream `main` at `2f4e82f`; `LLM_PROVIDER=mock` (`.env.example` default).

### Steps

With `API=http://127.0.0.1:8000`, from the repo root:

1. `cp .env.example .env && docker compose up -d db redis`
2. `python -m venv .venv && .venv/Scripts/pip install -e ".[dev]" && .venv/Scripts/alembic upgrade head && PYTHONIOENCODING=utf-8 .venv/Scripts/python scripts/seed_db.py` (I only need the API here, so I didn't install the frontend or the pre-commit hooks.)
3. `.venv/Scripts/uvicorn api.main:app --host 127.0.0.1 --port 8000`
4. In another terminal, `.venv/Scripts/python listener.py 20`, which logs any POST to `127.0.0.1:9999` for 20 s:

   ```python
   import sys, threading
   from datetime import datetime
   from http.server import BaseHTTPRequestHandler, HTTPServer

   SECONDS = int(sys.argv[1]) if len(sys.argv) > 1 else 20

   class Handler(BaseHTTPRequestHandler):
       def do_POST(self):
           body = self.rfile.read(int(self.headers.get("Content-Length", 0)))
           print(f"[{datetime.now():%H:%M:%S.%f}] POST {self.path} {body.decode(errors='replace')}", flush=True)
           self.send_response(200)
           self.end_headers()

       def log_message(self, *args):
           pass

   server = HTTPServer(("127.0.0.1", 9999), Handler)
   print("listening on 127.0.0.1:9999", flush=True)
   threading.Timer(SECONDS, server.shutdown).start()
   server.serve_forever()
   print(f"listener closed after {SECONDS}s", flush=True)
   ```

5. Get the seeded profile ID (there is no list-profiles route):
   `docker compose exec -T db psql -U pathreview -d pathreview_dev -tAc "SELECT p.id FROM profiles p JOIN users u ON u.id = p.user_id WHERE u.email = 'user1@example.com';"` → `fe1307a6-1c48-4c51-80ea-972d2c4ce401`
6. Log in (form-encoded, seeded credentials from `docs/SETUP.md`):
   `TOKEN=$(curl -s -X POST $API/auth/login -d "username=user1@example.com&password=password1" | python -c "import json,sys; print(json.load(sys.stdin)['access_token'])")`
7. Create a review with a `callback_url`, poll `GET /reviews/{id}/status` 5 times, then try `POST /webhooks` and `POST /reviews/{id}/webhooks`, all with `-H "Authorization: Bearer $TOKEN"`.

### What I observed

`/openapi.json` has no webhook or callback route:

```
POST /auth/register, POST /auth/login, POST /profiles, GET|PUT|DELETE /profiles/{profile_id},
POST /reviews, GET /reviews, GET /reviews/{review_id}, GET /reviews/{review_id}/status, GET /health, GET /
```

The review was accepted, and `callback_url` was silently dropped (`ReviewCreate` only has `profile_id`):

```
$ curl -s -w '\n  HTTP %{http_code}' -X POST $API/reviews -H "Authorization: Bearer $TOKEN" -H 'Content-Type: application/json' -d '{"profile_id": "fe1307a6-1c48-4c51-80ea-972d2c4ce401", "callback_url": "http://127.0.0.1:9999/hook"}'
  {"id":"825f1dd3-c755-48b4-bb18-ea23f5057922","profile_id":"fe1307a6-1c48-4c51-80ea-972d2c4ce401","status":"pending","sections":null,"overall_score":null,"error_message":null,"created_at":"2026-10-05T13:47:36.758226Z","updated_at":"2026-10-05T13:47:36.758226Z"}
  HTTP 200
```

It completed, which I could only find out by polling:

```
  [23:47:37.292] {"review_id":"825f1dd3-c755-48b4-bb18-ea23f5057922","status":"complete","progress_pct":0}
```

Nothing reached the listener, and both webhook routes are 404:

```
listening on 127.0.0.1:9999
listener closed after 20s

POST /webhooks                                              -> HTTP 404
POST /reviews/825f1dd3-c755-48b4-bb18-ea23f5057922/webhooks -> HTTP 404
```

`process_review()` in `core/services/review_service.py` sets `status = "complete"`, commits, and logs. It has no notification step, and `grep -rni webhook` finds nothing in the Python code.

### Outcome

I reproduced the missing capability: a client can't register a callback URL, nothing is sent on completion, and polling is the only option. I could **not** reproduce the 30–90 s processing time. With `LLM_PROVIDER=mock`, this review completed in about 119 ms (`created_at` …36.758 → `updated_at` …36.877). Testing the long-review case will need a real provider or an artificial delay.


---

## Eval iterations

### Run history

The calibration run for `calib-02` agreed with the gold label: `reject`. It was a calibration package and therefore counted as 0/0 scored items.

The first complete scored run agreed on 18 of 20 packages. The final complete run saved to `eval-run.txt` also agreed on 18 of 20 scored packages.

### Package analysis

For `pkg-09`, my rubric decided `reject`, while the gold label was `accept`. My rubric rejected the package because the `behavior-matched` check failed. The check required the evidence to match the reported problem closely, including the relevant syntax, punctuation, command, input, and symptom. This made my rubric read the package as showing an insufficiently exact match even though the gold label considered the reproduction acceptable.

### Check rationale

I used the following check in `rubric.md`:

> Pass if the evidence shows the same reported problem. The relevant syntax, punctuation, command, input, and symptom must match when they affect the result. A nearby error, different input, successful run, or unrelated failure does not pass.

I chose this wording because the worksheet feedback showed that a similar error is not necessarily the same issue. In particular, changing syntax such as `=` to `:` can produce a different error, so the rubric should compare the actual trigger and symptom instead of accepting any technically related failure.

### Trade-offs

The strict `behavior-matched` check helped the rubric correctly identify all four wrong-target packages, which contributed to the `wrong-target 4/4` category result. The trade-off was that it was too strict for `pkg-09` and `pkg-10`, which were both accepted by the gold labels but rejected by my rubric. Overall, the rubric still reached the required 18/20 agreement score, while prioritizing protection against posting evidence for the wrong behavior.
---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
