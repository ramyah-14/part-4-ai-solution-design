# AI Solution Design Report
## Manufacturing Defect Detection Using Computer Vision

---

## Task 1: Business Domain
**Domain:** Manufacturing

---

## Task 2: Business Problem Definition

### What problem is being solved?
Manufacturing plants produce thousands of products daily that must be
inspected for surface defects before shipping. Common defects include
dents, scratches, and stains that affect product quality and customer
satisfaction.

### Who are the users and stakeholders?
- **Primary users:** Quality control supervisors and line operators
- **Stakeholders:** Plant managers, operations directors, customers,
  and compliance teams

### What is the current process?
Human visual inspectors physically examine each product coming off the
assembly line. They look for visible surface defects and either approve
the product for packaging or flag it for rejection or rework.

### Limitations of the current process?
- Human inspectors fatigue over long shifts, leading to inconsistent
  decisions
- Inspection speed is a bottleneck — humans cannot keep up with fast
  production lines
- High labour cost for round-the-clock inspection teams
- Subjective judgement varies between inspectors
- No digital record of inspection decisions for quality audits

---

## Task 3: AI Task Type

**Selected AI Task:** Image Classification (Multi-class)

### Why is this appropriate?
Each product image needs to be assigned to exactly one of four categories:
normal, dent, scratch, or stain. This is a classic multi-class image
classification problem. The input is a photograph of the product surface
and the output is a single class label. CNNs are specifically designed
for this type of task and have proven highly accurate on visual pattern
recognition.

---

## Task 4: Data Requirement Plan

### Type of data needed
- Photographs of product surfaces taken under consistent lighting
  and camera angle conditions

### Structured or unstructured?
- **Unstructured** — raw image files (PNG or JPEG format)

### Input features
- Pixel values of product surface images
- Images resized to a fixed dimension (e.g. 64x64 or 128x128 pixels)
- Normalised pixel values between 0 and 1

### Target variable
- Defect class label: `normal`, `dent`, `scratch`, or `stain`

### Data collection method
- Install cameras at the end of the production line
- Capture one image per product as it passes
- Human inspectors label the first batch of images to create training data
- Over time, the model's predictions are verified and added back to
  the training set (active learning)

### Data quality risks
- Inconsistent lighting conditions may confuse the model
- Camera angle variation between production shifts
- Class imbalance if certain defects are rare
- Labelling errors in the initial human-annotated training set

---

## Task 5: Model Recommendation

**Recommended Model:** Convolutional Neural Network (CNN)

### Architecture
- 2 Convolutional layers with ReLU activation
- MaxPooling layers after each convolution
- Flatten layer
- Dense hidden layer
- Softmax output layer with 4 classes

### Why CNN?
CNNs are purpose-built for image data. Unlike regular neural networks,
CNNs use shared filters that detect local patterns (edges, textures,
shapes) regardless of where they appear in the image. This makes them
far more efficient and accurate for visual defect detection. Our
prototype CNN achieved 97.92% accuracy on the synthetic dataset,
correctly classifying 94 out of 96 test images.

### Future enhancement
Once sufficient real production data is collected, transfer learning
using a pre-trained model such as ResNet or EfficientNet would further
improve accuracy, especially for subtle defects.

---

## Task 6: Evaluation Plan

### Technical metrics
| Metric | Target |
|--------|--------|
| Test Accuracy | > 95% |
| Precision per class | > 90% |
| Recall per class | > 90% |
| Confusion Matrix | Diagonal dominant |

### Business metrics
| Metric | Description |
|--------|-------------|
| Defect escape rate | % of defective products that passed inspection |
| False rejection rate | % of good products incorrectly flagged |
| Inspection throughput | Number of products inspected per minute |
| Cost per defect caught | Compared to manual inspection cost |

### Possible failure cases
- Model misclassifies a dent as normal → defective product ships to customer
- Model misclassifies normal products as defective → good products wasted
- Model degrades over time as product design changes

### Human review process
- All flagged defects above a confidence threshold are auto-rejected
- Borderline predictions (confidence between 70-90%) are routed to a
  human inspector for final decision
- Weekly review of misclassified samples to retrain the model

---

## Task 7: Responsible AI Considerations

### Bias in data
If the training images are collected only during day shifts under
specific lighting, the model may perform poorly during night shifts
or in different factory locations. Training data must represent all
operating conditions.

### Incorrect predictions
A false negative (defective product classified as normal) is more
harmful than a false positive. The system should be tuned to have
higher recall for defect classes, even at the cost of some false
alarms, to protect customers.

### Privacy concerns
This solution uses only product images, not images of people, so
privacy risk is low. However, if worker activity is captured
incidentally by production line cameras, a data governance policy
must ensure those images are not stored or used for surveillance.

### Over-reliance on AI
Plant managers must not assume the model is infallible. Regular
audits comparing AI decisions against human expert review are
essential, especially after any changes to the product design or
production process.

### Impact on workers
Automated inspection may reduce the need for human inspectors.
The organisation should plan for workforce redeployment into
higher-value roles such as model monitoring, data labelling,
and quality engineering rather than simple job elimination.

### Need for human oversight
The model must operate within a human-in-the-loop framework,
especially during the initial deployment phase. No fully automated
rejection decision should be made without a defined escalation path
for edge cases.

---

## Task 8: Final Solution Summary

| Section | Detail |
|---------|--------|
| **Problem** | Manual visual inspection of manufactured products is slow, inconsistent, and expensive |
| **AI Solution** | CNN-based image classifier deployed at the production line to detect surface defects in real time |
| **Required Data** | Labelled product surface images across four classes: normal, dent, scratch, stain |
| **Model** | Convolutional Neural Network (CNN) with future upgrade path to transfer learning (ResNet/EfficientNet) |
| **Expected Business Impact** | 97%+ inspection accuracy, 10x faster throughput than manual inspection, 24/7 consistent quality control |
| **Key Risk** | False negatives (defects reaching customers) — mitigated by confidence thresholding and human review for borderline cases |
| **Mitigation Plan** | Human-in-the-loop review, weekly model retraining, diverse training data collection across all operating conditions |