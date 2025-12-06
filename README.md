# PV-SOLAR-PANEL
This project establishes a comprehensive, automated pipeline to assess the presence and area of solar Photovoltaic (PV) panels on rooftops based on geographic coordinates, generating auditable reports. 
 
​1. 🎯 Purpose and Mandate

​The primary mandate of this system is to facilitate the large-scale automated detection, classification, and area quantification of solar photovoltaic (PV) installations on residential and commercial rooftops.
​The system integrates geospatial data retrieval (Google Static Maps API) with proprietary Machine Learning (ML) inference capabilities to deliver high-confidence, auditable results, crucial for energy planning, asset assessment, and regulatory compliance

Key Configuration Parameters

​System integrity and operational scope are defined by the following constants:
​GOOGLE_STATIC_API_KEY: The institutional access credential for the satellite imagery service.
​ZOOM_LEVEL (19): Set for optimal rooftop detail (high-resolution context).
​IMAGE_SIZE (600x600): Defines the standardized input size for the Inference Engine.
​Buffer Zones: Define the area of interest (AOI) around the input coordinates, measured in square meters (derived from 1200 ft^2 and 2400 ft^2 approximations). These govern the scope of the PV search.

Input Requirements
​The input manifest must adhere to a strict schema for :

Field Name.  Description. 
Data Type  Requirement
sample_id  Unique Institutional Asset Identifier.  Integer/String Mandatory

latitude.   WGS84 Coordinate (Y-axis). Float       Mandatory

longitude.  WGS84 Coordinate (X-axis). Float.      Mandatory


Workflow Execution Stages



​Preparation:
​Set the INPUT_FILE (path to the manifest) and OUTPUT_DIR (repository location).
​Ensure all Python dependencies are installed.
​Fetch & Validation (Steps 1 & 4): The system generates a URL and attempts to download the JPEG image. A QC Status is immediately flagged if the image fetch fails (e.g., network error, API credential failure).
​Inference & Quantification (Steps 2 & 3):
​The downloaded image is fed to the (simulated) run_pv_inference function.
​The function returns:
​Boolean PV Presence.
​Estimated Area in m^2.
​A Detection Mask (polygon coordinates or bounding box).
​A Confidence Score (\in [0.0, 1.0]).
​Artifact Generation (Step 5): Two primary artifacts are produced for each run:
​Input Image: _input.jpg
​Result JSON: A structured file detailing all output metrics.

Output Repository Standard


​The OUTPUT_DIR serves as the audit trail. All outputs are designed for non-repudiation and downstream analysis.
​ PV Detection Result Schema
​Each resulting JSON file ({sample_id}_result.json) contains the definitive institutional record for the analysis:
​qc_status: Final determination on data reliability (VERIFIABLE or NOT_VERIFIABLE).
​pv_present: The classification result (Boolean).
​estimated_pv_area_sq_m: The core quantified metric.
​confidence_score: The statistical assurance of the detection.
​explanation: Contains the audit details, including the image filename, detection coordinates (detection_mask_or_bbox), and the full execution logic.
