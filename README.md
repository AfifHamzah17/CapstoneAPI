# Capstone API

REST API for a plant-disease detection app: JWT auth, image upload, and on-device
TensorFlow inference to classify a plant photo.

**Node.js · Express · TensorFlow.js (`tfjs-node`) · Firestore · Cloud Storage · Docker**

---

## Endpoints

| Method | Route | Purpose |
|--------|-------|---------|
| `POST` | `/auth/register` | Create an account (bcrypt-hashed password) |
| `POST` | `/auth/login` | Returns a JWT |
| `GET`  | `/profile` | Current user profile (auth required) |
| `POST` | `/detection` | Upload a plant photo, returns the predicted disease |
| `GET`  | `/plant` | Disease reference data |

A Postman collection (`Capstone API Test.postman_collection.json`) covers every route.

---

## Project structure

```
src/
├── app.js               Express app and route registration
├── routes/              auth · detection · plant · profile
├── services/
│   ├── loadModel.js     Loads the TF.js model at boot
│   ├── mlService.js     Image → prediction
│   ├── storeData.js     Writes results to Firestore/Storage
│   └── getAllData.js    Reads reference data
├── middlewares/         auth · uploadMiddleware (multer)
├── exceptions/          ClientError · InputError
└── utils/               Shared helpers
```

---

## Running locally

```bash
npm install
npm run start-dev    # nodemon
npm start            # production
npm run lint
```

### Environment

Create a `.env` with your Firebase service-account credentials and bucket name
(see `.env` keys referenced in `src/`). Firestore and Cloud Storage must be reachable
from the machine you run this on.

---

## Deployment

`Dockerfile` targets **Google Cloud Run** (`PORT=8080`, `node:18-slim`):

```bash
gcloud run deploy capstone-api --source .
```
