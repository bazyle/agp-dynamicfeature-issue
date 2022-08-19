# agp-dynamicfeature-issue

This is a sample project to show how to reproduce the duplicate dependency issue while packaging an
with multiple dynamic feature modules.
This project uses the default Android Studio template:
```
Android Studio Chipmunk | 2021.2.1 Patch 1
Build #AI-212.5712.43.2112.8609683, built on May 18, 2022
Runtime version: 11.0.12+0-b1504.28-7817840 aarch64
VM: OpenJDK 64-Bit Server VM by JetBrains s.r.o.
macOS 12.4
GC: G1 Young Generation, G1 Old Generation
Memory: 4096M
Cores: 10
Registry: external.system.auto.import.disabled=true, ide.instant.shutdown=false
Non-Bundled Plugins: detekt (1.21.0-RC2), PlantUML integration (5.15.2), idea.plugin.protoeditor (212.5080.8), org.jetbrains.kotlin (212-1.7.10-release-333-AS5457.46), by.overpass.svg-to-compose-intellij (0.3), pl.gmat.screengenerator (1.2.1)
```

To reproduce, simply run the following command:
`./gradlew clean bundleDebug` in terminal.

The output of that command(the error) is:
```text

* What went wrong:
Execution failed for task ':app:checkDebugLibraries'.
> [:dynamicfeature1, :dynamicfeature2] all package the same library [pub.devrel:easypermissions].
  
  Multiple APKs packaging the same library can cause runtime errors.
  Placing each of the above libraries in its own dynamic feature and adding that
  feature as a dependency of modules requiring it will resolve this issue.
  Libraries that are always used together can be combined into a single feature
  module to be imported by their dependents. If a library is required by all
  feature modules it can be added to the base module instead.

```

The "duplicated" dependency is `pub.devrel:easypermissions:3.0.0`, but shouldn't make a difference
on which dependency is used.
