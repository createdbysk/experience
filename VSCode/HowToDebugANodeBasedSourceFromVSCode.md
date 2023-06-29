# How to debug node.js based source in VSCode?
## Attach to process
- Configure `.vscode/launch.json` with the configuration below.
```json
{
    "version": "1.0.0",
    "configurations": [
      {
        "type": "node",
        "request": "attach",
        "name": "Attach to Process",
        "processId": "${command:PickProcess}",
        "env": { 
          "NODE_ENV": "test",
          // Other env variables here
          "LOG_LEVEL": "debug",
        },
        "envFile": "${workspaceFolder}/.env.testing"
      }
    ]
}
```
- Select the debug configuration above.
- Start your node process.
- Press `F5` or "run" the above configuration from the `Run and Debug` pane.
- Select the process to attach to.
- Note: You can use the same attach to process even to debug tests if you start your tests with `node --inspect-brk` followed by the actual test runner and its arguments.