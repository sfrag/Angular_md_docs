
## Anguar promises $q

A la hora de trabajar con promises existen una serie de pasos

  #### 1: Crear el objeto deferred
    
    - El objeto deferred controla el estado de la promise. Pude obtener 3 estados:
        resolve
        reject
        notify
        
    Así es como lo crearemos:
    
      var deferred = $q.defer();
    
  #### 2: Configurar la promise
  
    - El objeto promise es una propiedad del objeto deferred
    
      var promise = deferred.promise;
      
    promise.then(fnSuccess)
      .catch(fnFailure)  //opcional
      .finally(fnAlways) //opcional
      
   promise.then(fnSuccess, fnFailure, fnNotification)
    .catch(fnFailure)
    .finally(fnAlways)
    
#### 3: Resolución de la promise

  deferred.resolve(resultObj);  //fnSuccess
  deferred.reject(reasonObj);   //fnFailure
  deferred.notify(progressObj); //fnNotification
  

Uso básico de una promise:

    $http.get('movies.json')
    .then(function(response){
       $scope.movies= response.data;
    })
    .catch(function(response){
      console.log(response.status);
    });

Uso avanzado de una promise:

app.factory("Movies", function($http, $q) {
  return {
    get: function() {
        var deferred = $q.defer();
        $http.get('movies.json')
        .then(function(response){
           deferred.resolve(response.data);
        })
        .catch(function(response){
          deferred.reject(response);
        });
        return deferred.promise;
    }
  }
})
...
app.controller("MoviesCtrl", function($scope, Movies) {
   Movies.get().then(function(data){
      $scope.movies = data;
   })
   .catch(function(response){
      console.log(response.status);
   }); 
})







  
  
  
