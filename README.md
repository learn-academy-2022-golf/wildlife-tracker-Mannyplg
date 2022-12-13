# README

Wildlife Tracker Challenge
The Forest Service is considering a proposal to place in conservancy a forest of virgin Douglas fir just outside of Portland, Oregon. Before they give the go ahead, they need to do an environmental impact study. They've asked you to build an API the rangers can use to report wildlife sightings.

Story 1: In order to track wildlife sightings, as a user of the API, I need to manage animals.

Branch: animal-crud-actions

Acceptance Criteria

Create a resource for animal with the following information: common name and scientific binomial
Can see the data response of all the animals
# 3.0.0 :006 > Animal.all
  Animal Load (1.7ms)  SELECT "animals".* FROM "animals"
 =>                                                           
[#<Animal:0x00007fa2e851ab48                                  
  id: 1,                                                      
  name: "Lion",                                               
  binomial: "Panthera leo",                                   
  created_at: Tue, 13 Dec 2022 19:50:51.213454000 UTC +00:00, 
  updated_at: Tue, 13 Dec 2022 19:50:51.213454000 UTC +00:00>,
 #<Animal:0x00007fa2e851a8c8                                  
  id: 2,                                                      
  name: "Cat",                                                
  binomial: "Felis catus",                                    
  created_at: Tue, 13 Dec 2022 19:51:32.986669000 UTC +00:00, 
  updated_at: Tue, 13 Dec 2022 19:51:32.986669000 UTC +00:00>,
 #<Animal:0x00007fa2e851a788                                  
  id: 3,
  name: "Dog",
  binomial: "Cannis familiaris",
  created_at: Tue, 13 Dec 2022 19:52:43.972710000 UTC +00:00,
  updated_at: Tue, 13 Dec 2022 19:52:43.972710000 UTC +00:00>,
 #<Animal:0x00007fa2e851a6c0
  id: 4,
  name: "Horse",
  binomial: "Eqqus caballus",
  created_at: Tue, 13 Dec 2022 19:53:17.570293000 UTC +00:00,
  updated_at: Tue, 13 Dec 2022 19:53:17.570293000 UTC +00:00>,
 #<Animal:0x00007fa2e851a5f8
  id: 5,
  name: "Rabbit",
  binomial: "Leporidae cuniculas",
  created_at: Tue, 13 Dec 2022 19:54:09.174687000 UTC +00:00,
  updated_at: Tue, 13 Dec 2022 19:54:09.174687000 UTC +00:00>]

# Able to read instances (i.e. Horse)
GET-->http://127.0.0.1:3000/animals/4
{
    "id": 4,
    "name": "Horse",
    "binomial": "Eqqus caballus",
    "created_at": "2022-12-13T19:53:17.570Z",
    "updated_at": "2022-12-13T19:53:17.570Z"
}

Can create a new animal in the database
# TRANSACTION (0.5ms)  BEGIN
  Animal Create (1.1ms)  INSERT INTO "animals" ("name", "binomial", "created_at", "updated_at") VALUES ($1, $2, $3, $4) RETURNING "id"  [["name", "Dolphin"], ["binomial", "Delphinidae delphis"], ["created_at", "2022-12-13 21:20:55.041759"], ["updated_at", "2022-12-13 21:20:55.041759"]]          
  TRANSACTION (1.6ms)  COMMIT                                             
 =>                                                                       
#<Animal:0x00007f8af400fc68                                   
 id: 6,                                                       
 name: "Dolphin",                                             
 binomial: "Delphinidae delphis",                             
 created_at: Tue, 13 Dec 2022 21:20:55.041759000 UTC +00:00,  
 updated_at: Tue, 13 Dec 2022 21:20:55.041759000 UTC +00:00> 

Can update an existing animal in the database
# Animal Load (0.4ms)  SELECT "animals".* FROM "animals" WHERE "animals"."id" = $1 LIMIT $2  [["id", 1], ["LIMIT", 1]]
  ↳ app/controllers/animals_controller.rb:32:in `update'
  TRANSACTION (0.2ms)  BEGIN
  ↳ app/controllers/animals_controller.rb:33:in `update'
  Animal Update (0.6ms)  UPDATE "animals" SET "name" = $1, "binomial" = $2, "updated_at" = $3 WHERE "animals"."id" = $4  [["name", "Lion"], ["binomial", "Panther leo"], ["updated_at", "2022-12-13 22:28:27.541195"], ["id", 1]]
  ↳ app/controllers/animals_controller.rb:33:in `update'
  TRANSACTION (0.8ms)  COMMIT
  ↳ app/controllers/animals_controller.rb:33:in `update'
Completed 200 OK in 38ms (Views: 0.3ms | ActiveRecord: 11.6ms | Allocations: 5318)

Can remove an animal entry in the database
# Started DELETE "/animals/6" for 127.0.0.1 at 2022-12-13 13:59:39 -0800
Processing by AnimalsController#destroy as */*
  Parameters: {"id"=>"6"}
  Animal Load (0.2ms)  SELECT "animals".* FROM "animals" WHERE "animals"."id" = $1 LIMIT $2  [["id", 6], ["LIMIT", 1]]
  ↳ app/controllers/animals_controller.rb:23:in `destroy'
  TRANSACTION (0.2ms)  BEGIN
  ↳ app/controllers/animals_controller.rb:24:in `destroy'
  Animal Destroy (3.0ms)  DELETE FROM "animals" WHERE "animals"."id" = $1  [["id", 6]]
  ↳ app/controllers/animals_controller.rb:24:in `destroy'
  TRANSACTION (21.3ms)  COMMIT
  ↳ app/controllers/animals_controller.rb:24:in `destroy'
Completed 200 OK in 41ms (Views: 1.1ms | ActiveRecord: 28.8ms | Allocations: 4852)
#
Story 2: In order to track wildlife sightings, as a user of the API, I need to manage animal sightings.

Branch: sighting-crud-actions

Acceptance Criteria

Create a resource for animal sightings with the following information: latitude, longitude, date
Hint: An animal has_many sightings (rails g resource Sighting animal_id:integer ...)
Hint: Date is written in Active Record as yyyy-mm-dd (“2022-07-28")
Can create a new animal sighting in the database
Can update an existing animal sighting in the database
Can remove an animal sighting in the database

Story 3: In order to see the wildlife sightings, as a user of the API, I need to run reports on animal sightings.

Branch: animal-sightings-reports

Acceptance Criteria

Can see one animal with all its associated sightings
Hint: Checkout this example on how to include associated records
Can see all the all sightings during a given time period
Hint: Your controller can use a range to look like this:
class SightingsController < ApplicationController
  def index
    sightings = Sighting.where(date: params[:start_date]..params[:end_date])
    render json: sightings
  end
end
Hint: Be sure to add the start_date and end_date to what is permitted in your strong parameters method
Hint: Utilize the params section in Postman to ease the developer experience
Hint: Routes with params
Stretch Challenges
Story 4: In order to see the wildlife sightings contain valid data, as a user of the API, I need to include proper specs.

Branch: animal-sightings-specs

Acceptance Criteria
Validations will require specs in spec/models and the controller methods will require specs in spec/requests.

Can see validation errors if an animal doesn't include a common name and scientific binomial
Can see validation errors if a sighting doesn't include latitude, longitude, or a date
Can see a validation error if an animal's common name exactly matches the scientific binomial
Can see a validation error if the animal's common name and scientific binomial are not unique
Can see a status code of 422 when a post request can not be completed because of validation errors
Hint: Handling Errors in an API Application the Rails Way
Story 5: In order to increase efficiency, as a user of the API, I need to add an animal and a sighting at the same time.

Branch: submit-animal-with-sightings

Acceptance Criteria

Can create new animal along with sighting data in a single API request
Hint: Look into accepts_nested_attributes_for


