CongoMongo
===========

What?
------
A toolkit for using MongoDB with Clojure.

News
--------------
Version 0.1.7 (in progress) adds the ability to create MongoOptions
and pass them into make-connection as the last argument, so that you
can control autoConnectRetry and timeouts and so on.

Version 0.1.6 removes (almost) all of the reflection warnings.

Version 0.1.5 adds compatibility with both Clojure 1.3, in addition
to 1.2.

Congomongo 0.1.4 introduces support for the MongoDB 1.8's modified
map-reduce functionality, wherein the 'out' parameter is
required. With this and future Congomongo releases, it will no longer
be possible to access the map-reduce features of older MongoDB
instances.

As of congomongo 0.1.3, Clojure 1.2 and Clojure-contrib 1.2 are required.    
If you need compatibility with Clojure 1.1,     
please stick with congomongo 0.1.2.

There is now a [Google Group](http://groups.google.com/group/congomongo-dev)    
Come help us make ponies for great good.

Clojars group is congomongo.

=======

CongoMongo is essentially a Clojure api for the mongo-java-driver,
transparently handling coercions between Clojure and Java data types.

Basics
--------

### Setup

    (ns my-mongo-app  
      (:use somnium.congomongo))  
    (mongo!  
      :db "mydb") 

### Simple Tasks
------------------

#### create

    (insert! :robots    
             {:name "robby"}

#### read

    (def my-robot (fetch-one :robots)) => #'user/my-robot

    my-robot => { :name "robby", 
                  :_id  #<ObjectId> "0c23396f7e53e34a4c8cf400">, 
                  :_ns  "robots"}

#### update

    (update! :robots my-robot (merge my-robot { :name "asimo" }))

    =>  { :name "asimo" , 
          :_id  #<ObjectId> "0c23396f7e53e34a4c8cf400"> , 
          :_ns : "robots" }

#### destroy

    (destroy! :robots my-robot) => nil
    (fetch :robots) => ()

### More Sophisticated Tasks
----------------------------

#### mass inserts

    (mass-insert!  
      :points
      (for [x (range 100) y (range 100)] 
        {:x x 
         :y y 
         :z (* x y)})) 

     =>  nil

    (fetch-count :points)
    => 10000

#### ad-hoc queries

    (fetch-one
      :points
      :where {:x {:$gt 10  
                  :$lt 20}
              :y 42
              :z {:$gt 500}})

    => {:x 12, :y 42, :z 504,  :_ns "points", :_id ... }

#### easy json
------------------------------------------------------------------------

    (fetch-one :points 
               :as :json)

    => "{ \"_id\" : \"0c23396ffe79e34a508cf400\" , 
          \"x\" : 0 , \"y\" : 0 , \"z\" : 0 , \"_ns\" : \"points\"}"

   
Install
-------

Leiningen is the recommended way to use congomongo.
Just add 
    [congomongo "0.1.4-SNAPSHOT"]
to your project.clj and do
    $lein deps
to get congomongo and all of its dependencies.    

### Feedback

CongoMongo is a work in progress. If you've used, improved, 
or abused it tell us about it at our [Google Group](http://groups.google.com/group/congomongo-dev).
