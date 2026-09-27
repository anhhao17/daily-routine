# AGENTS.md — How to work with me

Guidance for AI coding agents (Claude Code, Codex, Cursor, …). I'm a senior
embedded engineer: C/C++, Zephyr, Linux kernel/drivers, Yocto, Jetson.
Follow these steps in order. Don't skip a step because the task looks small.

## 1. Understand the requirement
- Restate the requirement in 2–3 lines before doing anything: goal, target
  (board / SoC / OS / SDK version), and what "done" means.
- If something that changes the design is unclear (target HW, SDK version,
  API compatibility), ask. For small choices, take the sensible default and say so.

## 2. Research
- Official sources first: vendor docs, datasheets, reference manuals, upstream
  docs (kernel.org, docs.zephyrproject.org, docs.yoctoproject.org, NVIDIA docs).
  Then upstream source code and mailing lists. Blogs and forums last.
- Versions matter. Check the doc matches the version this project uses
  (JetPack/L4T, Yocto release, Zephyr version, kernel version).
- Cite the sources you used (URL + section) when you explain the solution.
- Don't invent registers, APIs, Kconfig symbols, or devicetree properties.
  If you can't find them in docs or source, say so.

### Hardware blocks and accelerators (ISP, VIC, NVENC/NVDEC, DLA, DMA, crypto, …)
- Read the vendor docs for the block first: developer guide, API reference,
  and limits (supported formats, resolutions, alignment, buffer/memory types).
- Find the vendor's sample app for it (e.g. Jetson Multimedia API samples,
  DeepStream / GStreamer samples, SDK examples) and read how it does setup,
  buffer handling, and teardown. Build and run the sample on the target first
  if possible, so we know the HW path works before our code is involved.
- Integrate into our project the way our code is structured. Use the sample
  to learn the correct call sequence, not as code to copy: rewrite it in our
  style, our classes, our error handling and logging. No copy-pasted sample
  code with its globals, `exit()` calls, or hardcoded paths.
- Keep the vendor API behind our existing abstraction if the project has one.
- Note in the explanation which sample/doc section the integration is based on,
  the SDK version, and any HW limits found (e.g. "input must be NV12,
  width aligned to N bytes", quoted from the doc).

## 3. Scan the project before writing code
- Read `CLAUDE.md` / `AGENTS.md` / `README` in the project, and check memory.
- Find how the project already solves similar things: grep for existing
  helpers, drivers, classes, recipes, and build patterns.
- Check `git log` for the files you will touch. Recent commits show the
  current style and why things are the way they are.
- List the files you plan to change before changing them.

## 4. Follow the project's style and rules
The project's existing style wins over everything below.

**C / C++**
- C++: SOLID, RAII, clear ownership (`std::unique_ptr` over raw `new`),
  `const`-correctness. But no abstraction without a second real user:
  no interface with one implementation, no factory for one product.
- Embedded: no dynamic allocation in ISRs or hard real-time paths, `volatile`
  only for HW registers, keep ISRs short, check every return code.
- Linux kernel code: follow kernel coding style (`checkpatch.pl`), `devm_*` APIs.

**Zephyr**
- Devicetree + Kconfig first, not hardcoded pins or addresses.
- Use Zephyr driver APIs and subsystems before writing custom code.

**Yocto**
- Follow well-maintained layers like `meta-tegra`, `meta-openembedded`, and poky.
- Never edit upstream layers. Change things with `.bbappend` in our own layer.
- Pin `SRCREV`, set correct `LICENSE` / `LIC_FILES_CHKSUM`, use `PACKAGECONFIG`
  and `DISTRO_FEATURES` over patches when possible.
- Patches go in `files/` with an `Upstream-Status:` header.

## 5. Implement and explain
- Reuse existing code. Do not add a new helper, class, or recipe if one
  already exists.
- If the existing code has a bug or limitation, fix it at its source so every
  caller benefits. Do not copy it and patch the copy.
- Keep the diff small and focused on the requirement. No unrelated refactors.
- After implementing, explain:
  - what changed and why (file by file, short)
  - what you reused, and what you fixed in the reused code
  - how to build and verify it (commands, what to look for on hardware)
  - risks and anything not tested (especially hardware you couldn't run on)
- Build or compile-check before saying it's done. If you couldn't, say so.

## Tone
- Be direct. Recommend one option, don't list every possibility.
- When something took long to debug, suggest a short debug note
  (symptom, root cause, fix, how to verify) for `docs/debug/`.
