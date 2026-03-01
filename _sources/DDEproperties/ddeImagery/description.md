## DDE Imagery Properties

Conditional extension for imagery resources. Implements the MD_Imagery entity from DDE spec Table 3. This building block is not included in the base DDEDiscovery profile — it is composed into the DDEImage profile when describing remote sensing or other imagery datasets.

All properties are optional per the DDE spec.

### Properties via `schema:additionalProperty`

- **`dde:sensorType`**: Type of sensor used to acquire the imagery (e.g., "Multispectral", "SAR", "LiDAR").
- **`dde:platform`**: Platform carrying the sensor (e.g., "Landsat-8", "Sentinel-2A", "Airborne").
- **`dde:equipment`**: Equipment used for imagery acquisition.
- **`dde:collector`**: Person or organization that collected the imagery.
- **`dde:signalGenerator`**: Type of signal used (e.g., "Passive solar", "Active radar").
- **`dde:wavelength`**: Wavelength range of the imagery (e.g., "0.45-0.52 micrometers").
- **`dde:processedLevel`**: Processing level of the imagery (value from ProcessedLevelCode, e.g., "Level-1", "Level-2A").

### Direct properties

- **`schema:startTime`**: Start time of the imagery acquisition (ISO 8601).
- **`schema:endTime`**: End time of the imagery acquisition (ISO 8601).
