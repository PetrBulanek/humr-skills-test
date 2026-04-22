---
name: hello-world
description: |
  A simple example skill for testing purposes. Greets the user, reports the current date/time, lists files in the working directory, and summarizes the project structure. Use to verify that skills are loading and executing correctly.
---

Greet the user warmly, then do the following steps in order:

1. Report today's date and current time (use the `date` command via Bash).
2. List the top-level files and directories in the current working directory.
3. If a `package.json`, `pyproject.toml`, `Cargo.toml`, or `go.mod` exists, read it and report the project name and language in one sentence.
4. Print a short summary: "Hello-world skill completed successfully."

Keep the entire response under 150 words.
