# escape=`
ARG BASE_IMAGE
ARG ASSETS_IMAGE

FROM $ASSETS_IMAGE as assets
FROM $BASE_IMAGE as build

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

ARG ASSETS_USE_WDP

COPY --from=assets ["${ASSETS_USE_WDP}", "C:\\temp\\packages\\"]

# expand selected wdp into installation directory
RUN Get-ChildItem 'C:\\temp\\packages\\*.zip' | Expand-Archive -DestinationPath 'C:\\temp'; `
    Copy-Item -Path 'C:\\temp\\App_Data\\jobs\\continuous\\AutomationEngine\\*' -Destination 'C:\\AutomationEngine' -Recurse -Force;

FROM $BASE_IMAGE

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

COPY --from=build ["C:\\AutomationEngine\\", "C:\\AutomationEngine\\"]
