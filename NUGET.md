## TestRift.NUnit

`TestRift.NUnit` streams NUnit test run events (logs, status, stack traces, attachments) to the **TestRift Server** and enables a real-time web UI for browsing and analysis.

### Experimental

TestRift is currently in an **experimental** phase. APIs, configuration, and data formats may change at any time **without notice**.

### Required: TestRift Server

You need the server running before your tests execute.

- **Server repo**: `https://github.com/testrift/testrift-server`
- **Run locally**:

```bash
pip install testrift-server
testrift-server
```

### Install (NuGet)

```bash
dotnet add package TestRift.NUnit
```

### Basic usage

Add the attribute and run hooks:

```csharp
using TestRift.NUnit;

[assembly: TRLogger]

[SetUpFixture]
public class MyRunHooks : RunHooks
{
}
```

### Configuration

Create a `TestRiftNUnit.yaml` file to configure the plugin (server connection, run metadata, grouping, and optional URL files).

The config is discovered from either:
- `TESTRIFT_NUNIT_YAML` (filesystem path), or
- `./TestRiftNUnit.yaml` in the current working directory.

All string fields support `${env:VAR_NAME}` expansion (missing variables expand to an empty string), which is useful in CI.

Example `TestRiftNUnit.yaml`:

```yaml
serverUrl: http://localhost:8080

runName: CI run ${env:GITHUB_RUN_NUMBER}
runId: ${env:GITHUB_RUN_ID}

metadata:
  - name: Firmware
    value: ${env:FIRMWARE_BRANCH}
  - name: CI
    value: ${env:GITHUB_RUN_ID}
    url: ${env:GITHUB_SERVER_URL}/${env:GITHUB_REPOSITORY}/actions/runs/${env:GITHUB_RUN_ID}

group:
  name: ${env:PRODUCT}
  metadata:
    - name: Branch
      value: ${env:BRANCH}

urlFiles:
  runUrlFile: test_run_url.txt
  groupUrlFile: test_group_url.txt
```

### Links

- **Repository**: `https://github.com/testrift/testrift-nunit`
- **Protocol reference**: `https://github.com/testrift/testrift-server/blob/main/docs/websocket_protocol.md`
- **Config reference**: `https://github.com/testrift/testrift-nunit/blob/main/docs/config.md`

