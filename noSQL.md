[noSQL course](https://openclassrooms.com/en/courses/4462426-maitrisez-les-bases-de-donnees-nosql)

# NoSQL --> Not Only SQL

NoSQL is interesting to manage the 3V (Volume, Velocity, Variety) rather than a relational database

## ACID vs BASE

A realtional DB follow the **ACID** properties : 
* Atomicité : Une transaction s’effectue entièrement ou pas du tout
* Cohérence : Le contenu d’une base doit être cohérent au début et à la fin d’une transaction
* Isolation : Les modifications d’une transaction ne sont visibles/modifiables que quand celle-ci a été validée
* Durabilité : Une fois la transaction validée, l’état de la base est permanent (non affecté par les pannes ou autre)

Theses properties can not be applied in a distributed context like NoSql because a syncronization between each server has to be done. 
So a NoSql base follow the **BASE** rules :
* Basically Available : quelle que soit la charge de la base de données (données/requêtes), le système garantie un taux de disponibilité de la donnée
* Soft-state : La base peut changer lors des mises à jour ou lors d'ajout/suppression de serveurs. La base NoSQL n'a pas à être cohérente à tout instant
* Eventually consistent : À terme, la base atteindra un état cohérent

## MongoDB
    
    db.restaurants.findOne()
    db.getCollection('restaurants').findOne()

```javascript
    db.restaurants.find( { "borough" : "Brooklyn" } )
    db.restaurants.find( { "borough" : "Brooklyn" } ).count()
    db.restaurants.find(
      {"borough":"Brooklyn",     
       "cuisine":"Italian",
       "name":/pizza/i,         //regex --> "pizza" not case sensitive
       "address.street" : "5 Avenue"},
      {"name" : 1,                  //projection --> show only specific keys
       "_id":0,
       "grades.score" : 1}
    )
```

[Query operators](https://docs.mongodb.com/v3.2/reference/operator/query/)

```javascript
db.getCollection('restaurants').find(
    {"borough":"Manhattan",
     "grades.score":{
         $lt:10, //less than 10
         $not:{$gte:10} //not greater than or equal 10
     }
    },
    {"name":1,"grades.score":1, "_id":0})
    
     db.restaurants.distinct("grades.grade");
     
     db.restaurants.aggregate([
        { $match : {
            "grades.0.grade":"C"
        }},
        { $project : {
            "name":1, "borough":1, "_id":0
        }},
        { $sort : { "name":1 }},
    ])
    
    varGroup = { $group : {"_id" : null, "total" : {$sum : 1} } };
    db.restaurants.aggregate( [ varMatch, varGroup ] );

    {"_id" : null, "total" : 220} //result
    
    varMatch = { $match : { "grades.0.grade":"C"} };
    varGroup3 = { $group : {"_id" : "$borough", "total" : {$sum : 1} } };
    db.restaurants.aggregate( [ varMatch, varGroup3 ] );
    
```

