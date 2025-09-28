Managing DVC Pipleline using DVC.YAML

Data processing stage

dvc stage add add -n data_process -p data_process.split_ratio -d source\data_process.py -d data\telecom_churn.csv -o processed_data python source\train.py 

Data training stage

dvc stage add -n train -p train.class_weight,train.max_depth,train.min_samples_leaf,train.min_samples_split,train.n_estimators -d processed_data\out_train.csv -d source\train.py -o model_dir\model.pkl python source\train.py processed_data\out_train.csv

Data testing stage

dvc stage add -n evaluate  -d source\evaluate.py -d model_dir\model.pkl -o results\precitions_vs_actuals.csv python source\evaluate.py processed_data\out_test.csv model_dir\model.pkl

DVC Queues

dvc exp run --name "num-of-trees-tuning"  --queue -S "train.n_estimators=50,100, 200, 300" -S "train.class_weight=balanced" 


dvc queue start 
dvc queue status

OR
dvc exp run --name "max-depth-tuning" --queue -S "train.n_estimators=50" -S "train.max_depth=2,9,10,12" -S "train.class_weight=balanced"

DVC Iterative Studio

<img width="1463" height="544" alt="image" src="https://github.com/user-attachments/assets/d19076f0-0827-476a-a927-8fb99da74313" />
