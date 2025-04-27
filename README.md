## IEEE-CIS Fraud Detection-Kaggle Competition
### მიმოხილვა:
  ეს პროექტი ეხებოდა თაღლითური ტრანზაქციების ამოცნობას უკვე არსებულ დატაზე დაკვირვებისა და დასწავლის დახმარებით. სხვადასხვა წყაროებიდან იყო ორი ცხრილი: თავად     ტრანზაქციების(590K) და ზოგიერთ ტრანზაქციაზე ასევე მოცემული იყო ინფორმაცია identityზე(140K). ქომფეთიშენის დეტალური აღწერა შეგიძლიათ იხილოთ [ლინკზე](https://www.kaggle.com/c/ieee-fraud-detection/overview)
### პრობლემის გადაჭრა
  *  პროექტზე მუშაობის დაწყებისას ძირითადი პრობლემა და გაურკვევლობა გამოიწვია იმან, რომ იყო ძალიან მწირი ინფორმაცია ფიჩერების შესახებ. ასევე, ბევრი იდეა მომივიდა, როგორ შეიძლებოდა ამ ორი სეტის გაერთიანება და უნდა მეპოვა ოპტიმალური ვარიანტი. საბოლოოდ, გადავწყვიტე რამდენიმე მოდელი დამეტესტა მხოლოდ ტრანზაქციების დატაზე და რამდენიმე მოდელი უკვე გაერთიანებულზე. როგორც ბოლოს ვნახე, დიდად სხვაობა არ მომცა ამ გაერთიანება/არ გაერთიანებამ. დასამერჯად გამოვიყენე ჩვეულებრივი left მერჯი, რომელიც ტრანზაქციებს მიამატებდა identityის ქოლუმებს და იმ როუებს, რომელზეც ამ უკანასკნელში ინფორმაცია არ ეწერა, შეავსებდა ნალებით. 
*  დატასეტის დაყოფა: train დატასეტი(ჯერ პირველ ორ მოდელში მხოლოდ transactions, ბოლო ორში დამერჯილი) გავყავი სამ ნაწილად: train, val and test, შესაბამისად: 70%, 15%, 15%. გავითვალისწინე, რომ იყო დაუბალანსებელი დატა, ამიტომ გაყოფისას ყველა ნაწილში მოვახვედრე თანაბარი რაოდენობის IsFraud ქოლუმნ დადებითი სემფლები.
### მიდგომები
* Nan მნიშვნელობების დამუშავება: რადგან დამერჯვით ბევრი ნალი მიჩნდებოდა, ვეცადე გამომეყენებინა სხვადასხვა ვარიანტები მათ შესავსებად. პირველი ეს იყო სტანდარტული მოდა/საშაულო, მეორე იყო სფეშელ ველიუები (ნუმერიქებისთვის -999, კატეგორიალებისთვის "Missing"). ასევე, მესამე ვარიანტი დაახლოებით ამათი გაერთიანებაა: თან შევცვალე მოდა/საშუალოთი, მეგრამ თან ამ მისინგნესის შესსანარჩუნებლად შემოვიტანე ახალი ქოლუმები, binary flags, რომლებშიც იწერებოდა რომელი როუში იყო მისინგ ველიუ ამა თუ იმ კრიტერიუმზე. 
* კატეგორიული ცვლადების რიცხვითში გადაყვანა: აქ ბევრი მიდგომა არ გამიტესტავს, კარგად ვერ ვიჭერ კორელაციას ამას და შედეგებს შორის, ამიტომ მხოლოდ სტანდარტული მიდგომა: one_hot_encoding იმ სვეტებისთვის, რომელშიც unique<3 და ordinary encoding დანარჩენებისთვის.
*  Feature Selection: ფიჩერების გასაფილტრად ძირითადად ვიყენებდი კორელაციის კლასს. ვცადე ასევე RFEის გამოყენებაც, მაგრამ ამხელა დატაზე იმდენად დიდი დრო დასჭირდა რომ მივხვდი არ გამომადგებოდა. ასევე, სამწუხაროდ, თითქმის ყველა ფიჩერი იყო "დაშიფრული" და კარგად ვერ ვხვდებოდი რა რას ახასიათებდა, ამიტომ გამიჭირდა ახალი ფიჩერების შემოტანა ძველების გაერთიანებით ან რაიმე მსგავსი. ამიტომ დამატებითი ფიჩერები არ გამომიყენებია. 

## რეპოზიტორიის სტრუქტურა
რაც შეეხება მოდელებს, სულ გამოვიყენე ოთხი მოდელი, ესენია: logistic regression, RandomForest, XGBoost, AdaBoost. ამათგან ყველაზე კარგი შედეგი მომცა XGBoostმა. ყველა მათგანისთვის მაქვს შესაბამისი სახელის ipynb ფაილი(model_experiment_Basic_Log_regression, model_experiment_Random_Forest_using_transactions,  model_experiment_AdaBoost_transactions+identity, model_experiment_XGBoost_transactions+identity). ქვემოთ ნახავთ ამ ყველა ფაილისთვის დეტალურ აღწერას, რომელ რანზე რა კლასებს და ჰიპერპარამეტრებს ვიყენებდი და როგორ ვაუმჯობესებდი შედეგებს. ასევე, model_inference_fraud_detection.ipynb-ში მაქვს საუკეთესო მოდელის გაშვება test.csvზე და მისი შედეგები. 

## Training
### model_experiment_Basic_Log_regression
დასაწყისისთვის, გავტესტე მარტივი ლოჯისტიკური რეგრესია მხოლოდ train_transactionის საშუალებით, სანამ საქმეში aidentityს ჩავრთავდი. 
* "initial run" *: მისინგები შევავსე მოდით/მედიანით(დაიდროპა სვეტები, სადაც მისინგები>90%), გამოვიყენე ordinal ენქოუდინგი(one_hot იმ სვეტებისთვის, სადაც იუნიქი<3). ასევე გავაკეთე კორელაციის დადროპვა. ვალიდაციაზე roc-auc მივიღე 0.74. 
* "saving_missing_values" *: შევცვალე მისინგების შევსება: ვივარაუდე, რომ შესაძლოა, მისინგნესი კორელაციაში იყოს დაფრედიქთებასთან, ამიტომ მისინგების პირდაპირ მოდით/მედიანით ჩანაცვლების მაგივრად, ჩავწერე მნიშვნელობები: -999/"MISSING"(დაიდროპა ქოლუმები, სადაც მისინგი >80%). ამან შედეგი უმიშვნელოდ გააუმჯობესა, მხოლოდ 0.1-ით და ავიდე 0.75ზე. 
* "saving all columns and its missing values" *: რადგან ვნახე, რომ მისინგ ველიუების "შენახვამ" დასწავლისას უკეთესი შედეგი გამოიღო, ვივარაუდე, რომ ის ქოლუმები, რომლებსაც ბევრი მისინგ ველიუ ჰქონდათ და ამის გამო ვშლიდი, შესაძლოა, პირიქით, დაგვხმარებოდა დასწავლის პროცესში, ამიტომ თრეშჰოლდი ავწიე 1ზე. ასევე, ფაიფლაინიდან ამოვიღე კორელაციის ფილტრი, რომ ყველა ქოლუმი შემენარჩუნებინა. ამან მომცა უკეთესი შედეგი: 0.79.
* "saving all columns and its missing values + scaler" *: აქამდე არ ვიყენებდი სქეილერს და დავამატე უბრალოდ სტანდარტ-სქეილერი და ვალიდაციის სქორი ავიდა 0.84მდე, რაც არის ამ ექსპერიმენტის საუკეთესო შედეგი. ამიტომ უკვე გავიშვი X_testზეც სამივე სეტის ქულა დავლოგე მეტრიკად(roc_auc_train = 0.8457 , roc_auc_val = 0.8446, roc_auc_test = 0.8431)

### model_experiment_Random_Forest_using_transactions
ამ მოდელში გავტესტავ Random Forestის მეთოდს, მაგრამ ჯერ კიდევ დაუმერჯავ დატაზე, მხოლოდ train_transactionების გამოყენებით. 
* "random forest with standart cleaning method" *: პირველ ვარიანტად დავწერე ყველაზე მარტივი რანდომ ფორესთი, რომელიც მისინგებს ავსებს მოდით/მედიანით და დროპავს ქოლუმებს, რომელშიც მისინგები > 80%. ენქოდერიც ასევე სტანდარტული, უნიკალური<3 ქოლუმებისთის one-hot, და დანარჩენებისთვის ordinal. ვალიდაციაზე ავიღე სქორი: 0.9126, მაგრამ თრენინგზე იყო 1.0, ამიტომ სავარაუდოდ მოხდა overfitting
* "less complex model with undersampling"  *: აქ შევცვალე მსინგების შევსების კლასი და გამოვიყენებ binary flagები, რომ თუკი მისინგნესი კორელაციაში იყო დასაფრედიქთებელ ქოლუმთან, მაინც შენარჩუნებულიყო ეს როგორც ფიჩერი. ასევე, ოვერფიტინგისგან თავის ასარიდებლად, შევამცირე მოდელის კომპლექსურობა(ნაკლები ხეები, ნაკლები სიღრმე, გავუზარდე min_samples_leaf). დავამატე ანდერსემფლინგი და კორელაციის ფილტრიც. roc_auc_train = 0.9204 , roc_auc_val = 0.9004. ტრეინზე დაიწია ქულამ, თუმცა ვალიდაცია მიახლოებულია წინა შედეგთან. მოდელი დიდად არ გაუარესდა და რაც მთავარია, ოვერფიტინგი შემცირდა.
* "using smoteSamoling, increased n_estimators": მოდელის გასაუმჯობესებლად შევცვალე სემფლინგის მიდგომა და underSamplingის მაგივრად გამოვიყენე Smote სემფლინგი. ასევე, გავზარდე n_estimator 100მდე. ასევე, მax_depth 0.6დან გადავაკეთე 'sqrt'ზე, რასაც, წესით ხეებს ააწყობდა ცოტა ნაკლები ფიჩერით და მივიღებდით უფრო მეტად განსხვავებულ ხეებს, ამიტომ ვივარაუდე, რომ ამასაც შეეძლო შედეგის გაუმჯობესება. დიდად არა, მაგრამ მოდელი ცოტათი გაუმჯობესდა: (roc_auc_train = 0.94 , roc_auc_val = 0.92, roc_auc_test = 0.91)

### model_experiment_AdaBoost_transactions+identity
ამ მოდელში გავტესტავ AdaBoost მეთოდს. აქ უკვე, შედეგის გასაუმჯობესებლად, დავმერჯე transactionები და identityის დატასეტები. გამოვიყენე left მერჯი, და რადგან 590K დან მხოლოდ 120K ტრანზაქციაში იყო identity ინფორმაცია, გაჩნდება ბევრი ახალი missing valueები, ამიტომ დატრენინგებისას, სავარაუდოდ დიდი მნიშვნელობა ექნება როგორ შევავსებ ამ მისინგებს და ვეცდები ხშირად შევცვალო და რამდენიმე სხვადასხვა ვარიანტი განვიხილო.
* "Basic version using AdaBoost": გამოვიყენე ყველაზე მარტივი DataCleaning, რომელიც მისინგებს ავსებს Mean/most freuqent-ებით. არ გამიწერია ბევრი პარამეტრი, მხოლოდ ცოტა n_estimator(50) და learning_rate(1). შედეგიც შესაბამისი მივიღე, train = 0.86, val = 0.858
* "using Spacial Values fot nan's": გავზარდე n_estimator და ოვერფიტინგი რომ არ მომხდარიყო, ცოტათი შევამცირე learning_rate. ასევე, AdaBoostს დავუმატე რამდენიმე პარამეტრი ინიციალიზაციისას და მისინგების შესავსებად გამოვიყენე მეორე მეთოდი, სფეშელ ველიუების(-999/Missing). სქორები ცოტა გაუმჯობესდა: train = 0.91, val = 0.9

### model_experiment_XGBoost_transactions+identity
ამ ფაილში ვტესტავ XGBoost მოდელს. აქაც, როგორც წინა მოდელში, დამერჯვის შემდეგ გამიჩნდა ბევრი ნალი, ამიტომ ვეცდები ხშირად ვცვალო მისინგების შევსების მიდგომა, რადგან წესით ამას გავლენა ექნება შედეგზე. 
* "Basic" *: ნალებს ვცვლი ჩვეულებრივ მოდა/საშუალო. ვიყენებ იგივე ენქოდერ კლასს. ჰიპერპარამეტრებში უმეტესად ვუსეტავ დეფოლტ მნიშვნელობებს. მივიღე 0.93 ტრეინზე და 0.92 ვალიდაციაზე. 
* "filling missings with special values + scaler + more n_estimator": მისინგების შესავსებად ვიყენებ სფეშელ ველიუებს: -999/"Missing". ჰიპერპარამეტრებში გავაკეთე რამდენიმე ცვლილება: n_estimators გავზარდე 500მდე უკეთესი პერფორმანსისთვის. subsample მცირედით გავზარდე ვარიენსის თავიდან ასაცილებლად და ასევე, რაგდან გვაქვს ძალიან არა ბალანსირებული დატა, დავამატე პარამეტრი scale_pos_weight, რომელიც მეტ ყურადღებას მიაქცევს პოზიტივ კლასს და ცოტა შემცირდება ეს იმბალანსი. ამ რანში ტრეინის სქორი ავიდა 0.99ზე და ვალიდაციისა ავიდა 0.95ზე. მართალია ვალიდაციის ქულაც ახლოსაა, მაგრამ მაინც საშიშია ოვერფიტინგი.
* "reduce overfitting, add underSamling and corelation dropper": როგორც ზემოთ ავხსენი, მაინც საშიშროება იყო ოვერფიტინგის,ამიტომ, უკეთესი ჯენერალიზაციისთვის დავამატე reg_alpha,(L1 რეგულარიზაცია) და reg_lambda(L2 რეგულარიზაცია). შევამცირე learning_rate. მივამატე კორელაცია და ანდერსემფლინგი. მისინგნესის შესანარჩუნებლად ვიყენებ binary flagებს. n_estimators დავაყენე 300ზე. ვალიდაციაზე ქულა ოდნავ შემცირდა, თუმცა ოვერფიტინგის ნიშნები აღარ იყო. (roc_auc_train = 0.96 , roc_auc_val = 0.94, roc_auc_test = 0.93)

## MLFlow Tracking
### [Basic_Log_regression](https://dagshub.com/zeliz22/ML_Fraud-Detection.mlflow/#/experiments/0?searchFilter=&orderByKey=attributes.start_time&orderByAsc=false&startTime=ALL&lifecycleFilter=Active&modelVersionFilter=All+Runs&datasetsFilter=W10%3D)
* [initial run](https://dagshub.com/zeliz22/ML_Fraud-Detection.mlflow/#/experiments/0/runs/3c26cb9e5a5742e2befd47f6e547349b)
* [saving_missing_values](https://dagshub.com/zeliz22/ML_Fraud-Detection.mlflow/#/experiments/0/runs/6336d57bfe834fffbda9e7c43bce359d)
* [saving all columns and its missing values](https://dagshub.com/zeliz22/ML_Fraud-Detection.mlflow/#/experiments/0/runs/699a7fe9ff1c442a90652a32c0d8947d)
* [saving all columns and its missing values + scaler](https://dagshub.com/zeliz22/ML_Fraud-Detection.mlflow/#/experiments/0/runs/e07ff089774543d7b9831fc0baa7bebf)
### [Random_Forest_using_transactions](https://dagshub.com/zeliz22/ML_Fraud-Detection.mlflow/#/experiments/1?searchFilter=&orderByKey=attributes.start_time&orderByAsc=false&startTime=ALL&lifecycleFilter=Active&modelVersionFilter=All+Runs&datasetsFilter=W10%3D)
* [random forest with standart cleaning method](https://dagshub.com/zeliz22/ML_Fraud-Detection.mlflow/#/experiments/1/runs/360ebd577ea647abbe287acf095c9bcd)
* [less complex model with undersampling](https://dagshub.com/zeliz22/ML_Fraud-Detection.mlflow/#/experiments/1/runs/706c6c8493514f9b88730b5146626261)
* [using smoteSamoling, increased n_estimators](https://dagshub.com/zeliz22/ML_Fraud-Detection.mlflow/#/experiments/1/runs/ba81f0007cc74d1d8fc171051874cf98)
### [AdaBoost_transactions+identity](https://dagshub.com/zeliz22/ML_Fraud-Detection.mlflow/#/experiments/3?searchFilter=&orderByKey=attributes.start_time&orderByAsc=false&startTime=ALL&lifecycleFilter=Active&modelVersionFilter=All+Runs&datasetsFilter=W10%3D)
* [Basic version using ](https://dagshub.com/zeliz22/ML_Fraud-Detection.mlflow/#/experiments/3/runs/a87656459f0f4060930b393dea04901e)
* [using Spacial Values fot nan's](https://dagshub.com/zeliz22/ML_Fraud-Detection.mlflow/#/experiments/3/runs/5596b90872d04c22a0b8c26fe90dce28)
### [XGBoost_transactions+identity](https://dagshub.com/zeliz22/ML_Fraud-Detection.mlflow/#/experiments/2?searchFilter=&orderByKey=tags.%60mlflow.runName%60&orderByAsc=false&startTime=ALL&lifecycleFilter=Active&modelVersionFilter=All+Runs&datasetsFilter=W10%3D)
* [Basic](https://dagshub.com/zeliz22/ML_Fraud-Detection.mlflow/#/experiments/2/runs/31c75f06c1694068b9c03d3f565b04b3)
* [filling missings with special values + scaler + more n_estimator](https://dagshub.com/zeliz22/ML_Fraud-Detection.mlflow/#/experiments/2/runs/396c2186cc6a46c4b319ef763e44521d)
* [reduce overfitting, add underSamling and corelation dropper](https://dagshub.com/zeliz22/ML_Fraud-Detection.mlflow/#/experiments/2/runs/afa4b7930255478f86d7b77b93ef31f0)





