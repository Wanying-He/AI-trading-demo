# AI-trading-demo

Features include

- An L1 contract which holds funds and exchanges WEth / USDC on Uniswap.
- An L2 contract implementing a simple (but flexible) 3-layer neural network for predicting future WEth prices.

## L2 Contract (Cairo Model)
The Cairo neural net model can be found in the `L2ContractHelper` directory, under the `L2RockafellerBot.cairo` file (pardon our misspelling!). 

## Getting Started
This directory is for all things training-related with respect to Rocky!

### Data Generation
Simply run `process_dataset.py` with no arguments. This command generates `.npy` files for the `playground_task` task which will be used in classification eval/training. 

The dataset currently generated for training/eval has as features the price (1 WEth --> USDC) difference between the current timestamp's price and 0-35 hours before. The label is the price bucket for price difference between the next hour's price and the current price.

### Model Training

```
python3 classification_train.py \
	--dataset playground_task \
	--model-type simple_3_layer_classifier \
	--num-epochs 100 \
	--model-name rockybot_sample_1
```

Results (saved model files, train stats, etc) are stored in `playground_task_task/simple_3_layer_classifier/rockybot_sample_1`.

### Model Validation
Similarly to the train command, we use argparse here. Example command is as follows:

```
python3 classification_eval.py \
	--dataset playground_task \
	--model-name rockybot_sample_1 \
	--model-type simple_3_layer_classifier
```

Enter the corresponding model checkpoint to load and evaluate. This command outputs the val loss and accuracy (note that these models will grossly overfit the training set, since market data is noisy and learning is ungeneralizable, as far as we can tell), and generates a confusion matrix as wellzq.
