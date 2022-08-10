# Configure Offline Build Dependencies - Android Studio
To build your project without a network connection, follow the steps below to configure Android Studio to use offline versions of the Android Gradle Plugin and Google Maven dependencies.
1. Download the offline components and unzip it's contents into the following Directory:
    * On Windows: ___%USER_HOME%/.android/manual-offline-m2/___
    * On macOS and Linux: ___~/.android/manual-offline-m2/___  
You'll have to create it if it doesn’t already exist.
2. Create an ___offline.gradle___ text file in the following path:
    * On Windows: ___%USER_HOME%/.gradle/init.d/offline.gradle___
    * On macOS and Linux: ___~/.gradle/init.d/offline.gradle___
3. Open the ___offline.gradle___ file and include the following script and *Save*:
```gradle
def reposDir = new File(System.properties['user.home'], ".android/manual-offline-m2")
def repos = new ArrayList()
reposDir.eachDir {repos.add(it) }
repos.sort()

allprojects {
  buildscript {
    repositories {
      for (repo in repos) {
        maven {
          name = "injected_offline_${repo.name}"
          url = repo.toURI().toURL()
        }
      }
    }
  }
  repositories {
    for (repo in repos) {
      maven {
        name = "injected_offline_${repo.name}"
        url = repo.toURI().toURL()
      }
    }
  }
}
```
* **NOTE**:
    * To update the offline components, You'll have delete the content inside the ___manual-offline-m2___ directory, download the Updated offline components and Unzip their contents into the ___manual-offline-m2___ directory.
    * This script applies to all Gradle projects you open or create on the workstation.
