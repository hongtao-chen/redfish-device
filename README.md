# Redfish Device Manager on WAC

## New Version
[Windows Admin Center](https://docs.microsoft.com/en-us/windows-server/manage/windows-admin-center/overview) released a [new version](https://docs.microsoft.com/en-us/windows-server/manage/windows-admin-center/understand/windows-admin-center) couple days back , and the [SDK](https://cloudblogs.microsoft.com/windowsserver/2018/09/20/windows-admin-center-1809-and-sdk-now-generally-available/) is now generally available in this version. Can we extend this Microsoft remote management platform to do things more than just Windows?

## Redfish
This DMTF standard (https://www.dmtf.org/standards/redfish) is supported by many hardware vendor to manage the device remotely, including many IoT devices. it exposes the service interface via a RESTful endpoint.

## Redfish on Windows Admin Center
If we can proxy the Redfish REST service via a [Window Admin Center gateway plugin](https://docs.microsoft.com/en-us/windows-server/manage/windows-admin-center/extend/guides/use-custom-gateway-plugin), and build a refish device [solution extension](https://docs.microsoft.com/en-us/windows-server/manage/windows-admin-center/extend/develop-solution) inside the Windows Admin Center UI, then we can manage not only the Windows, also the physical server that runs the Windows, and even other network devices.

## Putting things together

Windows Admin Center uses NUGET package as it's extension package format. So we need to build a nuget package for this redfish device extension.

**To use unsigned plugin, you must install WAC with DEV_MODE turned on, introduction is [here](https://docs.microsoft.com/en-us/windows-server/manage/windows-admin-center/extend/prepare-development-environment)**
```cmd
msiexec /i WindowsAdminCenter<version>.msi DEV_MODE=1
```

1. Create a `package` folder
2. Build the [gateway REST call forwarder project](https://github.com/hongtao-chen/wac-rest), create a `plugin` folder under the previous `package` folder and copy the `RestForwarder.dll` into it.
3. Build the [redfish device solution extension](https://github.com/hongtao-chen/wac-redfish-extension), copy the whole `public` folder under the `package` folder.

4. Create a nuget spec file in the `package` folder, below is an example:
```xml
<package xmlns="http://schemas.microsoft.com/packaging/2011/08/nuspec.xsd">
  <metadata>
    <packageTypes>
      <packageType name="WindowsAdminCenterExtension" />
    </packageTypes>  
    <id>redfish</id>
    <version>0.0.1</version>
    <title>Redfish Device Solution (Demo)</title>
    <authors>Hongtao Chen</authors>
    <owners>Hongtao Chen</owners>
    <requireLicenseAcceptance>false</requireLicenseAcceptance>
    <projectUrl>https://github.com/hongtao-chen/redfish-device</projectUrl>
    <description>Redfish device solution demo</description>
    <copyright>MIT</copyright> 
    <tags></tags>
  </metadata>
  <files>
    <file src="public\**\*.*" target="ux" />
    <file src="plugin\**\*.*" target="gateway" />
  </files>
</package>
```

5. run the command below to create the package
```bat
nuget pack redfish.nuspec
```
6. Copy the generated package `redfish.0.0.1.nupkg` into a network share, and add this share as a extension feed in WAC.

7. Install the `Redfish Device Solution (Demo)` from the WAC extension management UI.

Detail introduction about publishing WAC extension is here: https://docs.microsoft.com/en-us/windows-server/manage/windows-admin-center/extend/publish-extensions


