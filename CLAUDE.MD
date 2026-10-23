# CLAUDE.md — Multi-Agent Architecture exercises (Udacity, AgentCore)

## My role vs yours

I (Faith) write the exercise code myself — this is graded practice.
You are my reviewer, debugger and ops assistant:

- DO: review my code, explain errors, read logs, run local tests, suggest fixes as explanations or small diffs.
- DON'T: complete TODOs unless I explicitly say "write it".
- ASK before: deploying, any IAM/policy change, creating or deleting AWS resources, git push.
- NEVER: print or commit credentials, run `agentcore destroy` or delete resources unprompted.
- On failure: show the full error, explain the likely cause, stop. No improvised workarounds.

## Setting up a new lesson

- Copy ONLY the Exercise starter files from the raw downloads into a new numbered folder (e.g. 01-...).
  Never copy Demo or solution files; raw downloads are read-only reference.
- Create the lesson's own venv with uv (Python 3.13), install deps, report what was installed.
- Report facts about the starter (TODOs, entrypoints, expected payloads) without editing code.

## Environment

- Account/region: (TBD for this module), us-east-1. Credentials via `aws-env` (~/dev/AWS/load-aws-env.sh).
  If a token has expired, tell me — don't try to fix it.
- CLI: Python starter toolkit via `uv run agentcore ...`, never the global npm `@aws/agentcore`.
- Deploy with `--local-build` (CodeBuild blocked by the account SCP).

## Known gotchas (from the previous module)

- Naming: runtime/Memory names = underscores only; Gateway/targets/S3 = hyphens only.
- AccessDenied: each capability needs its own IAM permission, added incrementally.
  Multi-agent: one runtime invoking another needs bedrock-agentcore:InvokeAgentRuntime.
- Runtime deploy "Role validation failed" → check the execution role's trust policy.
- Payload shape: check whether the entrypoint reads "prompt" or "message"; orchestrator → sub-agent calls must match.
- Docker: check DOCKER_HOST / docker context if builds fail; watch file permissions in the container.
- Models re-derive numbers: have tools/sub-agents return final, pre-formatted figures.
- Shell: never leave <placeholders> in commands; `\` only at end of line.
- Budget: $25 total; run ~/dev/AWS/check-costs.sh regularly; shut down resources after each lesson.

## Logs

uv run agentcore status # shows the log group
aws logs tail "$LOG_GROUP" --since 15m | grep -E "Traceback|AccessDenied|ERROR"
