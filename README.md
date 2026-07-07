# BrainPlay content — deprecated

This repository previously served the app's question-bank packs (and a manifest)
over the GitHub / jsDelivr CDN, plus Spot the Difference scene images.

**It is no longer used.** BrainPlay now serves:

- **All question banks** from **Cloud Firestore** (`banks/{id}` + `questions` sub-collection).
- **Spot the Difference images** from **Firebase Cloud Storage**.

The CDN packs, manifest, and images have been removed. Git history is retained
if any content ever needs to be recovered.
