---
title: "Tokenizator"
date: 2022-05-07T12:49:39Z
---

## Restore Tokenizator Keys 

Tokenizator is a Google chrome app that allow you to generate MFA/ one time password with the same algo as google Authenticator,   
**:warning: WARNING :** is not meant for sensitive/production Keys , as mentioned by the Author :
> this application should not be used to generate sensitive codes because keeping them in your computer may cause a security issue, i recommend using this application only to test a service or to develop solutions that involve the Google Authenticator.

I'v been using Tokenizator for more than a year , haven't had any issues , easy UI & Usage .   
After some time it was time for me to change my machine, here i was stuck !!, as tokenizator has no way to export keys and i haven't found any workaround online ,  
 _PI : ( an issue is open for this matter )_ .  
So i had to find a Solution/workaroud for this issue , i started looking at the app code , available on [github](https://github.com/arosemena/Tokenizator) , checking for how the keys are stored .  
on the app Js file we have this function that looked interesting  :

```javascript

  $scope.saveToken = function(){
    if(typeof $scope.entry.secret !== 'undefined' && $scope.entry.secret.length > 0) {
      $scope.entry.secret = $scope.entry.secret.replace(' ', '');
      $scope.entry.key = auth($scope.entry.secret, epoch());
      $scope.appData.keys.push($scope.entry);
      $scope.page.active = 'password';
      $scope.entry = {};
      chrome.storage.local.set({'appData' : $scope.appData});
    }
  }

```
The keys are stored locally :thinking: , this relate with the author note `...keeping them in your computer...`
i stated looking for ways to retrieve stored values from local storage,  
On the way : 
*  I learned how to `inspect element` like on chrome apps  :
   * Go to `chrome://extensions/` and click on the desired app **Details**
   * Run the app and  find  **Inspect views** and choose the app . 
     ![Inspect_views](/Inspect_Views.png)
     ![Inspect_views2](/Inspect_Views1.png)

In order to restore the keys , we have to run js code on the running app, and `Inspect views` allows us to do so .  
now go to `console` and run 👇 to list the stored keys :

```javascript 
chrome.storage.local.get(null, function (data) { console.info(data) });
```
![Get_keys](/Restore_keys.png)

And congrats 🎉 you restored you keys , 
the keys will look smthg like : 
```json
{
    "keys": [
        {
            "key": "1****4",
            "name": "****_cloud",
            "secret": "R4O******************************NIB"
        },
        {
            "key": "0****4",
            "name": "APP1",
            "secret": "V**********************************C5"
        }
    ]
}
```
