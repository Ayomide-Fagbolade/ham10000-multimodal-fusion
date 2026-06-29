

EfficientNetV2-S pretrained on ImageNet was fine-tuned on HAM10000 using 224×224 resized images, ImageNet normalization, class-weighted cross-entropy, standard augmentation, and early stopping on validation macro-F1.

Best validation macro-F1: 0.837 at epoch 6.

Held-out test results:
- Accuracy: 0.8665
- Macro-F1: 0.8042
- Weighted-F1: 0.8679
- Balanced accuracy: 0.8342
