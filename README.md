# Redfish Device Manager on WAC

## New Version
[Windows Admin Center](https://docs.microsoft.com/en-us/windows-server/manage/windows-admin-center/overview) released a [new version](https://docs.microsoft.com/en-us/windows-server/manage/windows-admin-center/understand/windows-admin-center) couple days back , and the [SDK](https://cloudblogs.microsoft.com/windowsserver/2018/09/20/windows-admin-center-1809-and-sdk-now-generally-available/) is now generally available in this version. Can we extend this Microsoft remote management platform to do things more than just Windows?

## Redfish
This DMTF standard (https://www.dmtf.org/standards/redfish) is supported by many hardware vendor to manage the device remotely, including many IoT devices. it exposes the service interface via a RESTful endpoint.

## Redfish on Windows Admin Center
If we can proxy the Redfish REST service via a [Window Admin Center gateway plugin](https://docs.microsoft.com/en-us/windows-server/manage/windows-admin-center/extend/guides/use-custom-gateway-plugin), and build a refish device [solution extension](https://docs.microsoft.com/en-us/windows-server/manage/windows-admin-center/extend/develop-solution) inside the Windows Admin Center UI, then we can manage not only the Windows, also the physical server that runs the Windows, and even other network devices.
