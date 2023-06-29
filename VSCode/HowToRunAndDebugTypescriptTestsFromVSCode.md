# How To Run Typescript Tests from VSCode?
## Run Test
- Add the line below in `.vscode/settings.json`
- Change `MY_ENV`, `MY_ENV2` and the values to whatever environment variables you want to use.
```json
{
    "jest.jestCommandLine": "MY_ENV=env1 MY_ENV2=env2 npm test --"
}
```
## Debug Test
- Install the vscode-jest extension.
- Add the settings below in `.vscode/launch.json`
```json
    {
        "version": "1.0.0",
        "configurations": [
        // From https://marketplace.visualstudio.com/items?itemName=Orta.vscode-jest#debug-config
        {
            "type": "node",
            "name": "vscode-jest-tests.v2",
            "request": "launch",
            "program": "${workspaceFolder}/node_modules/.bin/jest",
            "args": [
            "--runInBand",
            "--watchAll=false",
            "--testNamePattern",
            "${jest.testNamePattern}",
            "--runTestsByPath",
            "${jest.testFile}",
            "--detectOpenHandles",
            "--coverage", "false",
            "--testTimeout", "500000"
            ],
            "cwd": "${workspaceFolder}",
            "console": "integratedTerminal",
            "internalConsoleOptions": "neverOpen",
            "env": { 
                // Whatever environment variables you need.
                "NODE_ENV": "dev"
            },
            // Env file.
            "envFile": "${workspaceFolder}/.env.testing",
            "sourceMaps": true,
            "disableOptimisticBPs": true,
            "windows": {
            "program": "${workspaceFolder}/node_modules/jest/bin/jest"
            }
        }
        ]
    }
```
  - You could also do it without the `vscode-jest` extension with the `launch.json` below. In this case, you must set the appropriate `<test name pattern regex>` for the `--testNamePattern` parameter below. The above extension makes it more interactive to achive the same result.
```json
    {
        "version": "1.0.0",
        "configurations": [
      {
        "type": "node",
        "request": "launch",
        "name": "Jest: current file",
        "env": { 
            "NODE_ENV": "test",
            "DB_MODEL_ENTITIES": "src/generated/models/db/entities/*.ts",
            "DATABASE_URL": "postgres://postgres:postgres@localhost:5432/delivery?sslmode=disable"
        },
        "envFile": "${workspaceFolder}/.env.testing",
        "program": "${workspaceFolder}/node_modules/.bin/jest",
        "runtimeArgs": ["--inspect-brk"],
        "args": [
            "${fileBasenameNoExtension}",
            "--config", "jest.config.js", 
            "--testNamePattern", "<test name pattern regex>",
            "--coverage", "false",
            "--detectOpenHandles",
            "--testTimeout", "500000",
          ],
        "console": "integratedTerminal",
        "windows": {
          "program": "${workspaceFolder}/node_modules/jest/bin/jest"
        }
      }
    ]
    }
```
