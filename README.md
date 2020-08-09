# GitHub Action For Running You Experiment With Allegro Trains

![GitHub stars](https://img.shields.io/github/stars/allegroai/trains?style=social)
![GitHub Workflow Status](https://img.shields.io/github/workflow/status/shomratalon/trains-train-model/Test%20train%20model)


Train models easily with Allegro Trains from your repo! 

## Usage
### Example

```yaml
name: Train model
on: [issue_comment]

jobs:
  train-model:
      if: contains(github.event.comment.body, '/train-model')
      runs-on: ubuntu-latest
      steps:
        - name: Train model
          uses: shomratalon/trains-train-model@master
          id: train
          with:
            TRAINS_API_ACCESS_KEY: ${{ secrets.ACCESS_KEY }}
            TRAINS_API_SECRET_KEY: ${{ secrets.SECRET_KEY }}
            TRAINS_API_HOST: ${{ secrets.TRAINS_API_HOST }}
            TASK_ID: "e4623efdfa1d461e9101615728fdc52e"
            QUEUE_NAME: "train_queue"
          env:
            GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        # Example how you can use outputs from the train action
        - name: Print task status
          run: |
            echo "Task stats is ${TASK_STATUS} for task ${CLONED_TASK}"
          env:
            TASK_STATUS: ${{ steps.train.outputs.TASK_STATUS }}
            CLONED_TASK: ${{ steps.train.outputs.CLONED_TASK }}
```

### Inputs

#### Mandatory Inputs
  1. `TRAINS_API_ACCESS_KEY`: Your trains api access key. You can find it in your trains.conf file under api.credentials.access_key section, [read more](https://allegro.ai/docs/references/trains_ref/#api-section). 
  2. `TRAINS_API_SECRET_KEY`: Your trains api secret key. You can find it in your trains.conf file under api.credentials.secret_key section, [read more](https://allegro.ai/docs/references/trains_ref/#api-section).
  3. `TRAINS_API_HOST`: The Trains api server address. You can find it in your trains.conf file under  api.api_server section, [read more](https://allegro.ai/docs/references/trains_ref/#api-section).
  4. `TASK_ID`: Id of the task you would like to clone.

#### Optional Inputs

  1. `QUEUE_NAME`: Queue for the cloned task. You can read more about queues [here](https://allegro.ai/docs/use_cases/trains_agent_use_case_examples/#running-workers).
  
### Outputs

1. `CLONED_TASK`: The cloned task id.
2. `TASK_STATUS`: The cloned task status (not updating).
