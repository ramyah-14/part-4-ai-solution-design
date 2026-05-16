# Part 4: AI Solution Design — Manufacturing Defect Detection

## Overview
This report presents an AI solution design for automated visual quality
inspection in a manufacturing environment, using a CNN-based image
classification model to detect surface defects in real time.

## Domain
Manufacturing — Surface Defect Detection

## AI Task Type
Multi-class Image Classification (normal / dent / scratch / stain)

## Files in This Folder
- `solution_report.md` — Full 8-task solution design report
- `diagrams/solution_architecture.png` — System architecture diagram

## Key Findings
- CNN is the recommended model architecture for this problem
- Prototype built in Part 2 achieved 97.92% test accuracy
- Human-in-the-loop review is essential for borderline predictions
- Transfer learning (ResNet/EfficientNet) recommended for production use

## Connection to Parts 1–3
This solution design is directly informed by the hands-on work done
in the earlier parts of this assignment:
- **Part 1** demonstrated how neural networks learn from tabular data
- **Part 2** built the actual CNN prototype on the defect image dataset
- **Part 3** showed how unstructured data (text/images) requires
  preprocessing before a model can learn from it