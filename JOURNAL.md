# Module 3 Journal

## Week 7 — Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/150

**Issue title:** Tech detector counts vendored and build-output files, skewing language detection

**Tier:** [x] Tier 1  [ ] Tier 2  [ ] Tier 3

**Problem summary:**
The agent system has a tool, `TechDetector` (in `agent/tools/tech_detector.py`),
that guesses a repository's tech stack from its list of file paths. It is supposed
to ignore third-party and generated code, but its skip logic only matches paths
wrapped in slashes (like `/node_modules/`), so relative paths such as
`node_modules/lib/index.js` or `build/bundle.js` slip through and get counted.
The result is that a project with 2 Python files and 6 bundled JS files is reported
as primarily JavaScript, which misrepresents the developer's actual skills. A
successful fix makes the detector reliably exclude vendored and build-output
directories regardless of whether the path is absolute or relative, so the primary
language reflects the author's own source code. Two existing tests,
`test_node_modules_excluded` and `test_build_directory_excluded`, should pass.

**Branch name:** fix/150-tech-detector-vendored-files

**Setup confirmation:** [x] App runs locally at localhost:5173

**Cohort ledger:** [ ] Issue added to cohort ledger

---

### "Is this right for me?" — selection notes

- **Scope fits Tier 1.** The change is contained to a single file
  (`agent/tools/tech_detector.py`) and its unit test file. No cross-module or
  architectural work — a good first contribution to a large codebase.
- **I understand the bug.** The `_should_skip_file` skip patterns require
  surrounding slashes, so relative vendored/build paths are not excluded. The fix
  is a matching-logic change, and there are already two failing tests defining the
  expected behavior.
- **Testable.** The issue gives an exact reproduction and names the two tests that
  should pass, so I can verify the fix objectively.
- **I originally claimed #102 (Tier 3, before/after comparison view),** but it is
  architectural and my first time in a codebase this size, so I stepped back and
  will do this Tier 1 first. If I finish before Week 10 I may pick up #102 as an
  optional second issue from a different tier.
