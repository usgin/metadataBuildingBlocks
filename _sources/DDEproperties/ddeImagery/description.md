## DDE Imagery Properties

Conditional extension for imagery resources. This building block is not included in the base DDEDiscovery profile — it should be composed into a DDEImageryProfile sub-profile when describing remote sensing or other imagery datasets.

Uses `schema:additionalProperty` with DDE-specific `propertyID` values:

### Required properties
- **`dde:sensorType`**: Type of sensor used to acquire the imagery (e.g., "Multispectral", "SAR", "LiDAR").
- **`dde:platform`**: Platform carrying the sensor (e.g., "Landsat-8", "Sentinel-2A", "Airborne").

### Optional properties
- **`dde:wavelengthRange`**: Wavelength range of the imagery (e.g., "0.45-0.52 micrometers").
- **`dde:signalGenerator`**: Type of signal used (e.g., "Passive solar", "Active radar").
- **`dde:processingLevel`**: Processing level of the imagery (e.g., "Level-1", "Level-2A").
