```
pool:
  vmImage: 'ubuntu-16.04' # Hosted Ubuntu 1604
  demands: maven

```

This section defines the pipeline's execution environment. 
It specifies that the pipeline should run on a hosted Ubuntu 16.04 image and that Maven should be available on the host machine.

```

steps:
- task: Maven@3
  displayName: 'Maven app/pom.xml'
  inputs:
    mavenPomFile: 'app/pom.xml'
    codeCoverageToolOption: Cobertura

- task: CopyFiles@2
  displayName: 'Copy Files to: $(build.artifactstagingdirectory)'
  inputs:
    SourceFolder: '$(system.defaultworkingdirectory)'
    Contents: '**/*.jar'
    TargetFolder: '$(build.artifactstagingdirectory)'
  condition: succeededOrFailed()

- task: PublishBuildArtifacts@1
  displayName: 'Publish Artifact: drop'
  inputs:
    PathtoPublish: '$(build.artifactstagingdirectory)'
  condition: succeededOrFailed()

```


This section defines the steps that should be executed in the pipeline. It consists of three tasks:

1. The **`Maven@3`** task which runs the Maven build process for the **`app/pom.xml`** file, using the Cobertura code coverage tool option.
2. The **`CopyFiles@2`** task which copies any **`.jar`** files produced by the Maven build to the **`$(build.artifactstagingdirectory)`** directory.
3. The **`PublishBuildArtifacts@1`** task which publishes the **`.jar`** files in the **`$(build.artifactstagingdirectory)`** directory as a build artifact named "drop".

The **`condition: succeededOrFailed()`** option for each task specifies that the task should always be executed, regardless of whether the previous tasks in the pipeline succeeded or failed.
