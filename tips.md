# Tips
## deployment
`git remote add alpha https://git.heroku.com/audyx-alpha.git`  
`git remote add beta https://git.heroku.com/audyx-beta.git`  
`git remote add production https://git.heroku.com/audyx.git`  
`git remote add us-production https://git.heroku.com/audyx-us.git`  

---
`git checkout branch`
`git push <environment_alias> master`
pushing differnet branches to different environments
`git push <environmmnnt_name> <branch_name>:master`


## alpha Deployment
---
1.  checkout develop branch
1.  update local branch   
    ``` git pull ```
1. make sure you are logged in with heroku cli  
    ```heroku login```
1. run deployment
    ```rake alpha deploy```

## Production deployment
---
* Deploy to procuction
1. checkout master branch
```git checkout master```  
``` git pull```  
1. Change version number by editing ```config/application.rb```  
```git commit -am "bump version number to <new_version_number>"```  
```git push```
1. run the deployment  
```bundle exec rake production deploy```

## Deploy to beta  
1. update the beta branch
```git checkout beta```
``` git pull```
1. checkout beta branch and merge develop
```git checkout develop```
```git pull```
```git checkout beta```
``` git merge develop```
1. merge master (in case of hotfixes in master **that haven't been merged yet**)
``` git checkout master```
```git pull```
```git checkout beta```
``` git merge master```
1. deploy beta
```git push```
```rake beta deploy```

## US deployment
1. checkout the branch `us-master` update and change version (config/application.rb)
```git checkout us-master```
```git pull```
```git commit -am "bump version number to <new_version_number>"```
1. update develop and merge
```git checkout develop```
```git pull```
```git checkout us-master```
```git merge develop```
```git push```
1. deploy to `us-production`
```rake us-prodcution deploy```

## Rebasing
When on your current branch, do the following:
1. `git checkout base-branch`
2. `git pull origin base-branch`
3. `git checkout your_working_branch`
4. `git rebase base-branch`
OR to cover up step 1,2,3,4, do this:
   `git pull --rebase origin base-branch`
5. Resolve conflicts in the files(if any)
If none(skip step 6). When you push, your pulled commits will just be automatically be added by git rebase.
6. `git add .`
7. There is no commiting here since git rebase does that automatically.
8. `git rebase --continue`
9. `git push origin branch-name -f`


## Cherrypicking from master to develop  
1. switch to master
2. pull 
3. note hash for commit
4. switch to develop
5. pull
6. cherry-pick hash
### in case of conflict:
7. fix merge errors
8. ```cherry-pick --continue```
9. push to develop

## [Ruby](https://guides.rubyonrails.org/)
### moving between versions
https://stackoverflow.com/q/37914702  

## ruby cli

### [migration](https://guides.rubyonrails.org/active_record_migrations.html)
```bin/rails generate migration AddPartNumberToProducts```


## working on koalys-services
1. run `DEV=true nodemon server.js` in services-audyx folder
2. in server env change to use local  change `SERVICES_AUDYX_URL=services-audyx-dev.herokuapp.com` to `SERVICES_AUDYX_URL=local.koalys.com:5001`
3. add somewhere in clojure the code to epxose the services for the console as a window object `(! js/window.theSocket socket)`
4. check from the console that it works data = `{room:"local#eb@audyx.com","user-email":"test.eb3@koalys.com",data:{}}`
`theSocket.emit("audyx::session::remote-calibration::exit",data)`
  
##  Checking heroku logs
---
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```heroku logs --tail -a [review-app]```
## Running local fig
---
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```lein fighweel [script]```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;i.e  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ```lein fighweel dev```

## ClojureScript
---
[Clojurescript cheatsheet](https://cljs.info/cheatsheet/ "Clojurescript cheatsheet")

### Clojurescript interop
[interop cfrom SO](https://stackoverflow.com/a/27061001)
```
(.method object) ; Equivalent to object.method()
(.-property object) ; Equivalent to object[property]

(.. object -property -property method)
(.. object -property -property -property)
```

### Dynamic access to object
```
(let [category (? $scope.test.yaml.category)
        steps (? $scope.test.base_yaml.loop.step.steps)
        default (? $scope.test.base_yaml.custom.category.|category|)])
```
        

### Vector to hashmap mcaro  
[SO answer](https://stackoverflow.com/a/45083049)
```
(defmacro ->hash [& vars]
  (list `zipmap
    (mapv keyword vars)
    (vec vars)))
```
    (->hash a b c) => {:a a :b b :c c}

## OM
---
[OM basic tutorial from wiki](https://github.com/omcljs/om/wiki/Basic-Tutorial "OM basic tuotrial")
## change state in runnig repl:
```
;; get to file
dev:cljs.user!{:conn 2}=> (in-ns 'om-tut.core)  
;; run commands from file
dev:om-tut.core!{:conn 2}=> (...)
;; change state
dev:om-tut.core!{:conn 2}=> (swap! app-state assoc :drek "dsfds")
```
## Some useful OM patterns :
### Retreiving component value via reference:  
Refd component:  
```
(dom/div nil
                        (dom/input #js {:type "text" :ref "new-contact"})
                        (dom/button #js {:onClick #(add-contact data owner)} "Add contact"))))))
```
Retreiving refd component value:  
```
(defn add-contact [data owner]
  (let [new-contact (-> (om/get-node owner "new-contact")
                        .-value
                        parse-contact)]
    (when new-contact
      (om/transact! data :contacts #(conj % new-contact)))))
```
# Misc
## creating super admin
```Superadmin.new(user_id:14, created_at: DateTime.now, updated_at: DateTime.now).save```

## finding patients
### cljs
```
 (defn id->hash [id]
  (->
    (* id 317)
    (+ 171)))
(defn hash->id [hash]
  (->
    (- hash 171)
    (/ 317)))
```  
### bash
```
chemiu@DESKTOP-7CHTJ2C:~$ more hashToId.sh
echo $((($1 -171) / 317))
chemiu@DESKTOP-7CHTJ2C:~$ . hashToId.sh 16272732
51333
chemiu@DESKTOP-7CHTJ2C:~$ more idToHash.sh
echo $(($1*317 -171))
chemiu@DESKTOP-7CHTJ2C:~$ . idToHash.sh 51333
16272390
```
### console
```localStorage.audyxPatient```

## QuickCalibration snippet
[addSnippet](https://developer.chrome.com/docs/devtools/javascript/snippets/)
```
// Define the keys
const keysToSmallerThanOne = ['[:audio :calibration :min-calibration-delta]',
'[:audio :calibration :tonal-duration-ratio]',
'[:audio :calibration :ramp-duration-in-sec]',
'[:audio :microphone :slot-durations :calibration]',
'[:audio :calibration :max-calibration-rounds]'
];
const maxKeyToChange = '[:audio :calibration :max-calibration-delta]';

//do the changes
const smallTime = 0.001;
const bigDelta = 150;

localStorage['[:audio :calibration :calibration-duration]']=1
localStorage[maxKeyToChange] = bigDelta;
keysToSmallerThanOne.map(key => {
  localStorage[key] = smallTime;
});
```
https://audio.koalys.com/?audio.microphone.mic-label&language=en#/audio-setup