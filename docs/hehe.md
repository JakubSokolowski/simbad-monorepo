# Data Models

### Artifact
```javascript
simulation: {
    id: 'id',
    startedUtc: '',
    finishedUtc: '',
    status: '',
    steps: {
        cli: {...},
        spark: {...},
        reports: {...}
    }
}

step: {
    id: '1231231',
    simulationId: '123123123',
    status: '',
    startedUtc: '',
    finishedUtc: '',
    status: '',
    executionInfo: {...}
    artifacts: [...],
    artifactIds: [...]
}

cliExecutionInfo: {
    memory: 34.5,
    cpu: 78.4,
    currentOutputSize: 1231412, 
}

artifact: {
    id: '',
    step: 'CLI | SPARK | REPORT'
    path: '/home/jakub/dev/out/cli.csv',
    size: '123123',
    createdUtc: '',
    simulationId: '123123'
}

```


