{
  "metadata": {
    "kernelspec": {
      "language": "python",
      "display_name": "Python 3",
      "name": "python3"
    },
    "language_info": {
      "name": "python",
      "version": "3.10.12",
      "mimetype": "text/x-python",
      "codemirror_mode": {
        "name": "ipython",
        "version": 3
      },
      "pygments_lexer": "ipython3",
      "nbconvert_exporter": "python",
      "file_extension": ".py"
    },
    "kaggle": {
      "accelerator": "none",
      "dataSources": [
        {
          "sourceId": 3136,
          "databundleVersionId": 26502,
          "sourceType": "competition"
        }
      ],
      "dockerImageVersionId": 30635,
      "isInternetEnabled": true,
      "language": "python",
      "sourceType": "notebook",
      "isGpuEnabled": false
    }
  },
  "nbformat_minor": 4,
  "nbformat": 4,
  "cells": [
    {
      "cell_type": "code",
      "source": "# This Python 3 environment comes with many helpful analytics libraries installed\n# It is defined by the kaggle/python Docker image: https://github.com/kaggle/docker-python\n# For example, here's several helpful packages to load\n\nimport numpy as np # linear algebra\nimport pandas as pd # data processing, CSV file I/O (e.g. pd.read_csv)\n\n# Input data files are available in the read-only \"../input/\" directory\n# For example, running this (by clicking run or pressing Shift+Enter) will list all files under the input directory\n\nimport os\nfor dirname, _, filenames in os.walk('/kaggle/input'):\n    for filename in filenames:\n        print(os.path.join(dirname, filename))\n# You can write up to 20GB to the current directory (/kaggle/working/) that gets preserved as output when you create a version using \"Save & Run All\" \n# You can also write temporary files to /kaggle/temp/, but they won't be saved outside of the current session",
      "metadata": {
        "_uuid": "8f2839f25d086af736a60e9eeb907d3b93b6e0e5",
        "_cell_guid": "b1076dfc-b9ad-4769-8c92-a6c4dae69d19",
        "execution": {
          "iopub.status.busy": "2024-01-23T17:38:00.873627Z",
          "iopub.execute_input": "2024-01-23T17:38:00.874100Z",
          "iopub.status.idle": "2024-01-23T17:38:00.884388Z",
          "shell.execute_reply.started": "2024-01-23T17:38:00.874068Z",
          "shell.execute_reply": "2024-01-23T17:38:00.883500Z"
        },
        "trusted": true
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": "train_data = pd.read_csv(\"/kaggle/input/titanic/train.csv\")\ntrain_data.head()",
      "metadata": {
        "execution": {
          "iopub.status.busy": "2024-01-23T17:31:29.956223Z",
          "iopub.execute_input": "2024-01-23T17:31:29.956631Z",
          "iopub.status.idle": "2024-01-23T17:31:29.984280Z",
          "shell.execute_reply.started": "2024-01-23T17:31:29.956598Z",
          "shell.execute_reply": "2024-01-23T17:31:29.982863Z"
        },
        "trusted": true
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": "test_data = pd.read_csv(\"/kaggle/input/titanic/test.csv\")\ntest_data.head()",
      "metadata": {
        "execution": {
          "iopub.status.busy": "2024-01-23T17:32:21.656248Z",
          "iopub.execute_input": "2024-01-23T17:32:21.656677Z",
          "iopub.status.idle": "2024-01-23T17:32:21.691525Z",
          "shell.execute_reply.started": "2024-01-23T17:32:21.656643Z",
          "shell.execute_reply": "2024-01-23T17:32:21.690093Z"
        },
        "trusted": true
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": "women = train_data.loc[train_data.Sex == 'female'][\"Survived\"]\nrate_women = sum(women)/len(women)\n\nprint(\"% of women who survived:\", rate_women)",
      "metadata": {
        "execution": {
          "iopub.status.busy": "2024-01-23T17:32:56.654584Z",
          "iopub.execute_input": "2024-01-23T17:32:56.655134Z",
          "iopub.status.idle": "2024-01-23T17:32:56.669518Z",
          "shell.execute_reply.started": "2024-01-23T17:32:56.655094Z",
          "shell.execute_reply": "2024-01-23T17:32:56.668164Z"
        },
        "trusted": true
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": "men = train_data.loc[train_data.Sex == 'male'][\"Survived\"]\nrate_men = sum(men)/len(men)\n\nprint(\"% of men who survived:\", rate_men)",
      "metadata": {
        "execution": {
          "iopub.status.busy": "2024-01-23T17:33:09.857380Z",
          "iopub.execute_input": "2024-01-23T17:33:09.857876Z",
          "iopub.status.idle": "2024-01-23T17:33:09.868319Z",
          "shell.execute_reply.started": "2024-01-23T17:33:09.857840Z",
          "shell.execute_reply": "2024-01-23T17:33:09.866764Z"
        },
        "trusted": true
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": "from sklearn.ensemble import RandomForestClassifier\n\ny = train_data[\"Survived\"]\n\nfeatures = [\"Pclass\", \"Sex\", \"SibSp\", \"Parch\"]\nX = pd.get_dummies(train_data[features])\nX_test = pd.get_dummies(test_data[features])\n\nmodel = RandomForestClassifier(n_estimators=100, max_depth=5, random_state=1)\nmodel.fit(X, y)\npredictions = model.predict(X_test)\n\noutput = pd.DataFrame({'PassengerId': test_data.PassengerId, 'Survived': predictions})\noutput.to_csv('submission.csv', index=False)\nprint(\"Your submission was successfully saved!\")",
      "metadata": {
        "execution": {
          "iopub.status.busy": "2024-01-23T17:34:02.176596Z",
          "iopub.execute_input": "2024-01-23T17:34:02.177132Z",
          "iopub.status.idle": "2024-01-23T17:34:03.553505Z",
          "shell.execute_reply.started": "2024-01-23T17:34:02.177087Z",
          "shell.execute_reply": "2024-01-23T17:34:03.552473Z"
        },
        "trusted": true
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": "",
      "metadata": {},
      "execution_count": null,
      "outputs": []
    }
  ]
}
