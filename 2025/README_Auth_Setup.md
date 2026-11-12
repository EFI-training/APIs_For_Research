# Google Places (New) API — Python Quick-Start Guide

This README provides a clean, step-by-step guide you can pair with screenshots. It walks you through:

* Creating a Google Cloud project
* Enabling billing
* Enabling the **Places (New)** API
* Creating & restricting an API key
* Installing the Python client
* Running real Place Details & Nearby Search examples

---

# 1. Create a Google Cloud Project

1. Open: [https://console.cloud.google.com](https://console.cloud.google.com)
2. Click the **Project Selector** (top nav) → **New Project**.
3. Choose a name and click **Create**.


---

# 2. Enable Billing

1. In the left menu, open **Billing**.
2. Attach or create a billing account.


---

# 3. Enable the Places (New) API

1. Open **APIs & Services → Library**.
2. Search for **“Places API”**.
3. Select the **Places API** entry.
4. Click **Enable**.


---

# 4. Create an API Key

1. Open **APIs & Services → Credentials**.
2. Click **Create Credentials → API Key**.
3. Copy the displayed API key.

---

# 5. Restrict the API Key (Recommended)

Open the key you created:

1. Under **Application restrictions**, choose:

   * HTTP referrers (web) **or**
   * IP addresses (server) **or**
   * App restrictions (Android/iOS)
2. Under **API restrictions**, choose **Restrict key** → select **Places API**.
3. Click **Save**.



---

# 6. Install the Python Client Library

```bash
pip install --upgrade google-maps-places
```

Optional environment setup:

```bash
python -m venv venv
source venv/bin/activate       # macOS / Linux
# .\\venv\\Scripts\\activate  # Windows PowerShell
```

---

# 7. Using the API in Python

## Authentication

Use an API key via an environment variable:

```bash
export PLACES_API_KEY="YOUR_API_KEY"       # macOS / Linux
# set PLACES_API_KEY=YOUR_API_KEY          # Windows
```

---

# 8. Python Example — Place Details (Async)

Create a file named `place_details_example.py`:

```python
import os
import asyncio
from google.maps import places_v1

API_KEY = os.environ.get("PLACES_API_KEY", "YOUR_API_KEY")

async def place_details(place_id: str):
    client = places_v1.PlacesAsyncClient(client_options={"api_key": API_KEY})
    name = f"places/{place_id}"

    request = places_v1.GetPlaceRequest(name=name)
    field_mask = "displayName,formattedAddress"

    response = await client.get_place(
        request=request,
        metadata=[("x-goog-fieldmask", field_mask)]
    )
    await client.close()
    return response

if __name__ == "__main__":
    example_place_id = "ChIJaXQRs6lZwokRY6EFpJnhNNE"  # Replace with a real Place ID
    result = asyncio.run(place_details(example_place_id))
    print(result)
```

Run it:

```bash
python place_details_example.py
```

---

# 9. Python Example — Nearby Search (Async)

Create `nearby_search_example.py`:

```python
import os
import asyncio
from google.maps import places_v1

API_KEY = os.environ.get("PLACES_API_KEY", "YOUR_API_KEY")

async def search_nearby(lat: float, lng: float, radius_m: int = 1000, place_type: str = "restaurant"):
    client = places_v1.PlacesAsyncClient(client_options={"api_key": API_KEY})

    circle_area = places_v1.SearchNearbyRequest.LocationRestriction.Circle(
        center=places_v1.LatLng(latitude=lat, longitude=lng),
        radius_meters=radius_m
    )
    location_restriction = places_v1.SearchNearbyRequest.LocationRestriction(circle=circle_area)

    request = places_v1.SearchNearbyRequest(
        location_restriction=location_restriction,
        included_types=[place_type]
    )

    field_mask = "places.displayName,places.formattedAddress,places.id"

    response = await client.search_nearby(
        request=request,
        metadata=[("x-goog-fieldmask", field_mask)]
    )
    await client.close()
    return response

if __name__ == "__main__":
    lat, lng = 51.5074, -0.1278  # Example coordinates (London)
    result = asyncio.run(search_nearby(lat, lng, radius_m=1500, place_type="restaurant"))
    print(result)
```

Run it:

```bash
python nearby_search_example.py
```

---

# 10. Notes on Field Masks

The new Places API **requires** choosing which fields you want returned.

* This is done via the `x-goog-fieldmask` header.
* Requesting only necessary fields reduces cost and improves performance.

Examples:

* Minimal fields: `displayName,formattedAddress`
* More fields: `places.displayName,places.id,places.businessStatus,places.location`

---

# 11. Best Practices

* **Do not** commit API keys to GitHub or share them in any public forum.
* Use environment variables or a secrets manager.
* Restrict keys (referrers/IP + API restrictions).
* Rotate keys periodically.
* Set up budget alerts in Google Cloud Billing.

---



---

# Finished!
