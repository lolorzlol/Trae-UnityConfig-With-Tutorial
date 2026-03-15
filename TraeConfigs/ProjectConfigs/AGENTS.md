# AGENTS.md

## Development Workflow

### Operation Requirements

- Check both `message` and `success` from MCP call results to determine success. When `message` doesn't match expectations, treat the call as failed.
- When MCP calls fail, adjust parameters based on the returned `message` and retry.
- Use `batch_execute` as much as possible to reduce MCP call times.
- Follow `Test-Driven Development (TDD)` during development.
- Check the Unity Editor `Console` log with `read_console` regularly for errors.
- Write editor scripts to config and setup scene rather than direct `unityMCP` call.
- If editor scripts is created for tasks, execute it `automatically` via MCP, avoid execute manually. The explict guide is in **GameObjects Setup & Editing & Configuration**.
- Use git for version control.
- Add Tooltip attributes to editor-configurable variables.

### Plan Mode Orchestration

- Non-trivial tasks (3+ steps or architecture decisions) must enter plan mode.
- If things go off track, stop and replan—don't push through.
- If bugs or frustration accumulate, stop and replan.
- Write detailed specs as input to reduce ambiguity.
- Use numbered steps.

### Task Management

1. Plan first: write plan to `tasks/todo.md` with checkboxes.
2. Validate plan before starting implementation.
3. Implement incrementally: check off each step as completed.
4. Explain changes: provide high-level summary for each step.
5. Document results: add review section in `tasks/todo.md`.
6. Record lessons: update `tasks/lessons.md` after corrections.

### Subagent Strategy

- Use subagents heavily to keep the main context window clean.
- Outsource research, exploration, and parallel analysis to subagents.
- Reduce cognitive load and separate concerns via subagents.
- One task per subagent, focused execution.

### Code Quality Principles

**Self-Improvement Loop:**

- After user correction: update `tasks/lessons.md` to record patterns.
- Write rules for yourself to prevent the same mistakes.
- Iterate improvements until error rate drops.
- Verify improvements each session, apply to relevant projects.

**Verification Defaults:**

- Never mark a task complete until you've proven it works.
- Compare main branch to your changes when relevant.
- Always be ready to push to production, verify.
- Ask yourself "Would a senior engineer approve this?"
- Do assertions, logs, test suite corrections.

**Elegant Code:**

- For non-trivial changes: pause and ask "Is there a more elegant way?"
- If a fix feels like a patch: "Now that I know everything, implement an elegant solution."
- Skip this step for simple obvious fixes, don't over-engineer.
- Challenge your own work before committing.

**Core Principles:**

- Simplicity first: each change should be as simple as possible, affecting minimal code.
- Don't be lazy: find root causes, no band-aid fixes, senior engineer standards.
- Minimal impact: changes should only involve what's necessary.

**Autonomous Bug Fixing:**

- When you find a bug: fix it directly, don't ask for help.
- Point out logs, errors, failing tests, then resolve.
- User doesn't need to switch context.
- Proactively fix failing CI tests, no need to tell how.

## Testing

### Test-Driven Development (TDD)

- Follow the *Red-Green-Refactor* cycle: write failing tests first, then implement code, and run tests again to make them pass.
- Run tests through `run_tests` in `unityMCP` with parameters:
  -`mode`: "EditMode" or "PlayMode"
  - `test_names`: Array of test names to run (optional, runs all if omitted)
  - `include_failed_tests`: Include test details for failed tests
- After tests pass, refactor code through code-review.

## Skills Reference

Load the appropriate skill before starting each workflow:

| Workflow | Skill |
|----------|-------|
| Using unityMCP | `unity-mcp-helper` |
| Using unity-test-framework | `unity-test-framework` |
| Test-Driven Development | `test-driven-development` |

## Preferences

### Input System

- Use the legacy input system.

### Git

- Do not use `git worktree`.
- Commit changes staged periodically.

## GameObjects Setup & Editing & Configuration

- Design the size and materials of different objects carefully.
- **Automated Configuration with Editor Scripts**: When needing to set up complex references, create editor scripts (in `Assets/Scripts/Editor/` folder) to automate the configuration process:
  - **Scene object references**: Use `GameObject.Find()` to locate objects in the scene.
  - **Prefab references**: Use `AssetDatabase.LoadAssetAtPath<T>()` to load prefabs and assets from disk.
  - Assign them directly to component fields programmatically. Add a menu item with `[MenuItem]` attribute to trigger the setup process.
- **Running Editor Scripts**:
  1. Use `[MenuItem("YourMenuPath")]` attribute on a static method in your editor script.
  2. Refresh Unity with `mcp_unityMCP_refresh_unity` to compile the script.
  3. Execute the menu item with `mcp_unityMCP_execute_menu_item` and pass the exact menu path.
  4. The script will run in the Unity editor and perform all configured actions.

## Troubleshooting

### Common Issues

- When using unity-test-framework, forgetting to call the `unity-test-framework` skill causes compilation errors.

### Debugging Steps

- First check the Console window for compilation errors.
- Check for any other issues as needed.