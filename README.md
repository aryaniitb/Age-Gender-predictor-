                ┌────────────────────────────────────────┐
                │          Dataset (Images)              │
                │   (faces with age + gender labels)     │
                └────────────────────────────────────────┘
                                  │
                                  ▼
                ┌────────────────────────────────────────┐
                │     Data Preprocessing (Transforms)    │
                │ Resize → Crop → Normalize (ImageNet)   │
                └────────────────────────────────────────┘
                                  │
                                  ▼
                ┌────────────────────────────────────────┐
                │          AgeGenderModel (ResNet50)     │
                │ - Backbone: ResNet50 (pretrained)      │
                │ - Attention Module (focus on faces)    │
                │ - Age Head: Regression (0–100 years)   │
                │ - Gender Head: Binary Classification   │
                └────────────────────────────────────────┘
                                  │
                        ┌─────────┴─────────┐
                        ▼                   ▼
             ┌───────────────────┐  ┌───────────────────┐
             │   Age Prediction  │  │  Gender Prediction │
             │ (MAE loss, scaled │  │ (BCE loss, sigmoid│
             │  0–1 → 0–100 y)   │  │ threshold 0.5)     │
             └───────────────────┘  └───────────────────┘
                                  │
                                  ▼
                ┌────────────────────────────────────────┐
                │       Training & Optimization           │
                │ Loss = Age Loss + Gender Loss           │
                │ Optimizer = AdamW                      │
                │ Scheduler = CosineAnnealingLR           │
                └────────────────────────────────────────┘
                                  │
                                  ▼
                ┌────────────────────────────────────────┐
                │       Model Evaluation (Holdout Set)   │
                │  - Age MAE (Mean Absolute Error)        │
                │  - Gender Accuracy, Confusion Matrix   │
                │  - Classification Report               │
                └────────────────────────────────────────┘
                                  │
                                  ▼
                ┌────────────────────────────────────────┐
                │      Inference / Deployment            │
                │                                        │
                │  1. Load Trained Model (ResNet+Heads)  │
                │  2. Use YOLOv8-face for face detection │
                │  3. Crop + Transform each face         │
                │  4. Predict Age + Gender               │
                │  5. Combine confidence scores          │
                │  6. Annotate & Save Results            │
                └────────────────────────────────────────┘
