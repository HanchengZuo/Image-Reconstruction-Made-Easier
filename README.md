# Inpainting-NormalMaps

---

### v2.0-Bowtie-EarlyStopping

**Version Description:** Removed generator skip connections from version `v1.2-U_Net-EarlyStopping`, generator uses bowtie-like architecture. The performance obtained by bowtie-like training is far inferior to that of the U-Net structure, see `v1.2-U_Net-EarlyStopping` of the test for a comparison.

**Key Component Summary:**

| Component                        | Brief Description                                                                                                     |
|----------------------------------|-----------------------------------------------------------------------------------------------------------------------|
| Gen Model Structure              | `Activation('tanh')` <br/>`UnitNormalizeLayer(L2+axis=2+epsilon=1e-6)`                         |
| Gen Loss Function                | λ_rec(`10`) * reconstruction_loss (`cosine similarity`) +<br/> λ_adv(`1`) * adversarial_loss (`binary cross-entropy`) |                                                                                              |
| Disc Model Structure             | `BatchNormalization()`<br/>`LeakyReLU(alpha=0.2)`<br/>`Activation('sigmoid')`                                         |
| Disc Loss Function               | `Binary Crossentropy(1,real_output)`+`Binary Crossentropy(0,fake_output)`                                             |
| Gen Optimizer<br/>Disc Optimizer | `Adam(1e-4)`<br/>`Adam(1e-4)`                                                                                         |
| Training Mask                    | Irregular Line Mask                                                                                                   |
| Training Method                  | Simultaneous training, updating gradients every `batches_list_num` batches. <br/>Training stops if no improvement in `test generator loss` for `patience`(25) epochs, only stop at `epoch_break` intervals. |

---

### v1.2-U_Net-EarlyStopping

**Version Description:** Added early stopping to `v1.0-U_Net`: training halts if no reduction in `test generator loss` within `patience` epochs, but stops only after model save post the next `epoch_break`.

**Comparative Analysis:** Outperforms `v1.0-U_Net` in all metrics and visual quality. EarlyStopping prevents overfitting observed in `v1.0-U_Net` results. EarlyStopping training at epoch `225` instead of `300` epochs in `v1.0-U_Net`.

**Key Component Summary:** Same as `v1.0-U_Net`, except for the training method.

| Component                        | Brief Description                                                                                                                                  |
|----------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|
| Training Method                  | Simultaneous training, updating gradients every `batches_list_num` batches. <br/>Training stops if no improvement in `test generator loss` for `patience`(25) epochs, only stop at `epoch_break` intervals. |

---

### v1.1-U_Net-EarlyStopping(train anomaly)

**Version Description:** Added early stopping to `v1.0-U_Net`, but with faulty logic. Post-epoch 143, generator loss surged, degrading performance. Theoretically, early stopping shouldn't affect training; cause unknown.

**Key Component Summary:** Same as `v1.0-U_Net`, except for the training method.

| Component                        | Brief Description                                                                                                                                  |
|----------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|
| Training Method                  | Simultaneous training, updating gradients every `batches_list_num` batches. <br/>Early stopping. |

---

### v1.0-U_Net

**Version Description:** Initial version.

**Key Component Summary:**

| Component                        | Brief Description                                                                                                     |
|----------------------------------|-----------------------------------------------------------------------------------------------------------------------|
| Gen Model Structure              | `SkipConnections` <br/>`Activation('tanh')` <br/>`UnitNormalizeLayer(L2+axis=2+epsilon=1e-6)`                         |
| Gen Loss Function                | λ_rec(`10`) * reconstruction_loss (`cosine similarity`) +<br/> λ_adv(`1`) * adversarial_loss (`binary cross-entropy`) |                                                                                              |
| Disc Model Structure             | `BatchNormalization()`<br/>`LeakyReLU(alpha=0.2)`<br/>`Activation('sigmoid')`                                         |
| Disc Loss Function               | `Binary Crossentropy(1,real_output)`+`Binary Crossentropy(0,fake_output)`                                             |
| Gen Optimizer<br/>Disc Optimizer | `Adam(1e-4)`<br/>`Adam(1e-4)`                                                                                         |
| Training Mask                    | Irregular Line Mask                                                                                                   |
| Training Method                  | Simultaneous training, updating gradients every `batches_list_num` batches. <br/>Specify number of `epochs` to train. |

---



## README for `train_test` Code Integration

### Functionality
- **Model Saving:** After completing the 'train' function call, the `train_test` code automatically saves the trained generator model in '.h5' format.
- **Resuming Interrupted Training:** In scenarios where the code execution is interrupted, rerunning the `train_test` code will pick up the training process from where it left off. The code prints out the previous training information and continues the training until the specified number of epochs is reached and the generator model is saved.
- **Training Completion Check:** If the generator model file already exists when the code is run, it implies that the training phase has been completed previously. In this case, the code will skip the training steps and proceed directly to the testing phase.

### Instructions for Use
1. **Initial Run:** Execute the `train_test` code. The training phase will begin, processing the data and adjusting the generator model.
2. **Interrupted Sessions:** If the training is interrupted, simply rerun the `train_test` code. It will resume training from the last saved state.
3. **Testing Phase:** Once the model is trained and saved, or if a pre-trained model is detected, the code will enter the testing phase to evaluate the model's performance.