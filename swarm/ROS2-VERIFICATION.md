# ROS 2 host verification (examples, not approved commands)
Use the same project-owned build/test entrypoint as GitHub Actions where practical. Record exact working directory, sourced underlay/overlay, compiler, ROS distro, environment and exit codes. Confirm test commands and package closure with the team. Do not run these examples blindly.

Illustrative commands from a configured isolated workspace:
```bash
colcon build --packages-up-to <package> --cmake-args -DBUILD_TESTING=ON
colcon test --packages-select <package> --return-code-on-test-failure --event-handlers console_direct+
colcon test-result --verbose
```
Use an approved targeted GTest invocation for RED/GREEN, capturing name, assertion and output. For RED prove the new test fails for the expected behavior, not a missing setup or broken build; document approved exceptions for characterization or compile-first work. For GREEN re-run the identical test. After refactoring, run affected package tests; final QA uses the full approved host regression command. Verify expected tests were discovered and actually executed; stale XML, disabled tests and zero-test success are not evidence. Use fresh isolated build/test result directories or approved cleanup procedure (never delete unrelated files). Record RED snapshot/diff separately from final candidate. Verify expected counts, failures, skipped tests and XML/log references. Test graph isolation, ROS_DOMAIN_ID and discovery settings must follow project policy; no connection to vehicle or shared production graph.

Evidence levels (never conflate):
- host unit: GTest/GMock, pure C++ and interface seams;
- host integration: approved isolated ROS graph/launch tests, if applicable;
- CI: independent GitHub Actions checks for actual PR SHA;
- target build: approved Thor toolchain/build provenance;
- target runtime: controlled hardware validation by authorized staff, with measured timing/resources if required.

If target verification is required but unavailable, record the gap and escalate to the appropriate owner. Do not automatically declare story acceptance or release readiness. No target flashing or live hardware testing by agents.

Reference: https://colcon.readthedocs.io/en/released/reference/verb/test.html and https://colcon.readthedocs.io/en/released/reference/verb/test-result.html
