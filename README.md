# Inpainting-NormalMaps

# README for `train_test` Code Integration

## Functionality
- **Model Saving:** Upon completing the training, the `train_test` code automatically saves the trained generator model. This ensures that the model can be reused without the need for retraining.
- **Training Completion Check:** If the generator model file already exists when the code is run, it implies that the training phase has been completed previously. In this case, the code will skip the training steps and proceed directly to the testing phase.
- **Resuming Interrupted Training:** In scenarios where the code execution is interrupted, rerunning the `train_test` code will pick up the training process from where it left off. The code prints out the previous training information and continues the training until the specified number of epochs is reached and the generator model is saved.

## Instructions for Use
1. **Initial Run:** Execute the `train_test` code. The training phase will begin, processing the data and adjusting the generator model.
2. **Post-Training:** After training, the model is saved for future use. The code automatically transitions to the testing phase if the model exists.
3. **Interrupted Sessions:** If the training is interrupted, simply rerun the `train_test` code. It will resume training from the last saved state.
4. **Testing Phase:** Once the model is trained and saved, or if a pre-trained model is detected, the code will enter the testing phase to evaluate the model's performance