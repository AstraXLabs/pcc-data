# PCC Data Repository

This repository contains scripts for fetching and processing Google Places API data for P.C. Chandra Jewellers locations.

## Setup

### 1. Install Dependencies
```bash
npm install
```

### 2. Environment Configuration
Create a `.env` file in the root directory and add your Google Maps API key:

```bash
cp .env.example .env
```

Then edit `.env` and add your actual API key:
```
GOOGLE_MAPS_API_KEY=your_actual_google_maps_api_key_here
```

**Important:** Never commit the `.env` file to git. It's already included in `.gitignore` for security.

## Available Scripts

### Fetch Place Details
```bash
npm run fetch-places
```
Fetches complete place details from Google Maps API for all locations in `placeIds.json`. Results are saved to `scripts/raw_place_details.json`.

### Convert to Photos-Review Format
```bash
npm run convert-reviews
```
Converts raw place details to a clean photos-review format. Processes only new IDs that haven't been converted yet. Results are saved to `scripts/photos-review.json`.

### Update Photos-Review Data
```bash
npm run update-reviews
```
Updates the `config/photos-review.json` file with fresh review data from raw place details. Features:
- Filters reviews to only include ratings >= 4
- Limits to 10 high-quality reviews per location
- Preserves existing photos data (photos are static)
- Removes domain from profile photo URLs
- Sorts reviews by rating and recency

### Fetch Fresh Reviews (NEW)
```bash
npm run fetch-fresh-reviews
```
**⭐ RECOMMENDED:** Fetches fresh review data directly from Google Places API using store data. Features:
- **Fetches live data** from Google Places API using `config/stores.json`
- Filters reviews to only include ratings >= 4
- Limits to 10 high-quality reviews per location
- Preserves existing photos data (photos are static)
- Removes domain from profile photo URLs
- Sorts reviews by rating and recency
- **Handles API restrictions** gracefully with fallback to cached data

**⚠️ API Key Requirements:** This script requires a Google Places API key without HTTP referrer restrictions. If your current API key has restrictions, the script will use cached data as fallback.

### Validate Photos-Review Data
```bash
npm run validate-reviews
```
Validates the quality and format of the `config/photos-review.json` file. Provides:
- Data structure validation
- Review quality metrics
- Rating distribution analysis
- Profile photo URL format checks
- Overall quality score

## Files Structure

- `scripts/placeIds.json` - Contains mapping of location names to Google Place IDs
- `scripts/raw_place_details.json` - Raw data from Google Places API
- `scripts/photos-review.json` - Processed data with reviews (photos excluded)
- `config/` - Configuration and processed data files
- `.env` - Environment variables (not tracked in git)
- `.env.example` - Template for environment variables

## Security Notes

- API keys are stored in environment variables
- The `.env` file is git-ignored for security
- Use `.env.example` as a template for required environment variables

## Scripts Description

- `fetch-new-raw-place.js` - Fetches place details from Google Maps API
- `convert-to-photos-review.js` - Converts raw data to processed format (initial conversion)
- `update-photos-review.js` - Updates config with fresh reviews (rating >= 4, max 10 per location)
- `fetch-fresh-reviews.js` - **NEW** Fetches live reviews directly from Google Places API using stores.json
- `download-place-photos.js` - Downloads photos using Google Places API
- `validate-photos-review.js` - Validates the quality and format of photos-review data