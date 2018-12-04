# escape=`

##
# Docker image to build Azure Service Fabric Projects in a container
##

##
# Install Visual Studio 2017 Build Tools
# Reference Documentation: https://docs.microsoft.com/en-us/visualstudio/install/build-tools-container?view=vs-2017
##

# Use the latest Windows Server Core image with .NET Framework 4.7.1.
FROM microsoft/dotnet-framework:4.7.1

# Restore the default Windows shell for correct batch processing below.
SHELL ["cmd", "/S", "/C"]

# Download the Build Tools bootstrapper.
ADD https://aka.ms/vs/15/release/vs_buildtools.exe C:\TEMP\vs_buildtools.exe

# Install Build Tools excluding workloads and components with known issues.

RUN C:\TEMP\vs_buildtools.exe --quiet --wait --norestart --nocache `
    --installPath C:\BuildTools `
    --add Microsoft.VisualStudio.Workload.ManagedDesktopBuildTools `
    --add Microsoft.VisualStudio.Workload.NetCoreBuildTools;includeOptional `
    --add Microsoft.VisualStudio.Workload.WebBuildTools `
    --add Microsoft.VisualStudio.Workload.AzureBuildTools;includeOptional `
 || IF "%ERRORLEVEL%"=="3010" EXIT 0

# Start developer command prompt with any other commands specified.
ENTRYPOINT C:\BuildTools\Common7\Tools\VsDevCmd.bat &&

# Default to PowerShell if no other command specified.
CMD ["powershell.exe", "-NoLogo", "-ExecutionPolicy", "Bypass"]

##
# Install Service Fabric SDK - the download links are avaiable in the Service Fabric Release Notes as Windows Developer Set-up and Windows .NET SDK 
##
ADD https://download.microsoft.com/download/D/D/D/DDD408E4-6802-47FB-B0A1-ECF657BEB35F/MicrosoftServiceFabric.6.4.617.9590.exe C:\TEMP\MicrosoftServiceFabricRuntime.exe

ADD https://download.microsoft.com/download/D/D/D/DDD408E4-6802-47FB-B0A1-ECF657BEB35F/MicrosoftServiceFabricSDK.3.3.617.msi C:\TEMP\MicrosoftServiceFabricSDK.msi

RUN C:\TEMP\MicrosoftServiceFabricRuntime.exe /accepteula /sdkcontainerclient /quiet

RUN msiexec.exe /i "C:\TEMP\MicrosoftServiceFabricSDK.msi" /qn 