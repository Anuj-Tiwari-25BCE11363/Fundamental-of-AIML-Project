# Sign-Link API Documentation

**Author:** Aarushi Jain | 25BAI11158 | VIT  
**Base URL (local):** `http://localhost:5000`

---

## POST `/api/predict`

Predict the ASL sign from a base64-encoded image frame.

**Request body (JSON):**
```json
{
  "image_base64": "<base64-encoded JPEG/PNG string>",
  "model": "cnn"
}
```

**Response (JSON):**
```json
{
  "prediction": "A",
  "confidence": 92.4,
  "recognised": true,
  "model_used": "cnn",
  "sign_probabilities": {
    "A": 92.4,
    "B": 4.1,
    "C": 1.8,
    "del": 0.6,
    "nothing": 0.7,
    "space": 0.4
  }
}
```

---

## GET `/api/stats`

Returns aggregate statistics over all logged predictions.

**Response (JSON):**
```json
{
  "total": 3840,
  "recognition_rate": 91.5,
  "avg_confidence": 88.3,
  "sign_distribution": {
    "A": 142, "B": 98, "C": 115, "...": "..."
  },
  "most_predicted_sign": "A"
}
```

---

## GET `/api/history?n=20`

Returns the `n` most recent prediction logs (default 10).

**Response:** Array of prediction log objects ordered newest-first.

---

## GET `/switch-model/<model_name>`

Hot-swap the active inference model.

**Supported values:**

| Model | Description |
|-------|-------------|
| `cnn` | CNN on raw image frames (default, highest accuracy) |
| `random_forest` | Random Forest on landmark features (interpretable) |
| `svm` | SVM on landmark features (good generalisation) |

---

## POST `/api/session/start`

Start a new recognition session; returns a `session_id`.

**Response:**
```json
{ "session_id": "sess_abc123", "started_at": "2025-11-15T10:30:00Z" }
```

---

## GET `/api/session/<session_id>`

Retrieve the full prediction log for a session, including the assembled sentence.

---

## Field Reference

| Field | Type | Values / Range | Notes |
|-------|------|---------------|-------|
| `image_base64` | string | base64 JPEG/PNG | Recommended frame size: 224×224 px |
| `model` | string | `cnn`, `random_forest`, `svm` | Defaults to `cnn` |
| `prediction` | string | A–Z, `del`, `nothing`, `space` | 29 possible classes |
| `confidence` | float | 0–100 | Probability of top class (%) |
| `recognised` | bool | true / false | False only for `"nothing"` class |

---

## Error Codes

| Code | Meaning |
|------|---------|
| 400  | Missing or invalid `image_base64` field |
| 404  | Session ID not found |
| 500  | Inference error (model not loaded) |
