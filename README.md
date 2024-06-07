# MongoDB Sandbox

## Installation de MongoDB Atlas

--> Suivre les étapes de la vidéo : https://www.youtube.com/watch?v=jXgJyuBeb_o retranscrites ci-dessous :

- Try Free -> créer un compte
- Remplir le formulaire et sélectionner "Javascript/Node.js" pour le langage.
- Prendre la version *gratuite*
- Changer le si besoin, Name : Cluster0 -> ClusterSimplonTest
- Laisser le reste par défaut
Cliquer sur : Create Cluster

Sur l'écran suivant : 
- Choisir Authentification par Username/Password, et sauvegardez vos identifiants/mdp (c'est important !!!)
- Puis cliquer sur "Create User"
- Ensuite, cliquer sur "Add My Current IP Address"
- Puis cliquer sur "Finish and Close"

### Import des données
- Sur l'écran suivant, cliquer sur "Load sample Data", et patienter pendant la création du dataset.
- Une fois le dataset chargé, cliquer sur "Browse Collection"


### Intégration avec VSCode ou Compass

Dans le menu "Database", cliquer sur "Connect", et choisir "Compass", et copier le "connection string".
A partir de là, il est possible d'utiliser VSCode ou Compass avec le même accès.

__VS Code :__

Pour VSCode, télécharger l'extension "MongoDB for VS Code", installer, puis cliquer sur l'icône en feuille de MongoDB sur la gauche et dans "Add Connection", coller le "connection string", ne pas oublier de remplacer le mot de passe par celui défini précédemment.

Vous avez maintenant accès au différentes bases de données, vous pouvez voir les documents, etc. 
Et il y a même un "playground" avec des exemples, dans le quel vous pouvez tester des requêtes (c'est là qu'on va travailler).

Regarder l'exemple du Playground, et le lancer avec l'icône "Run" en haut à droite.
Observer les résultats, et raffraichir les collections pour voir apparaitre la nouvelle base de données.

Tuto VSCode : https://youtu.be/MLWlWrRAb4w

__Compass :__

Télécharger et installer Compass. 
Dans "Add Connection", coller le "connection string", ne pas oublier de remplacer le mot de passe par celui défini précédemment. 

Pour la suite, on va utiliser Mongosh, cliquer tout en bas de la fenêtre de Compass pour l'ouvrir. C'est une sorte de console.

Tuto Compass : https://www.youtube.com/watch?v=YBOiX8DwinE

## Exercices 

Pour les exercices, on va travailler dans la base de données `sample_airbnb`, 

__Pour VSCode__

Créer un nouveau fichier, comme le playground. 
Et faire un `use('sample_airbnb');` pour commencer. 
La base de données est ensuite accessible avec `db. ...`

__Pour Compass__

Dans la console Mongosh, taper : 
```
use sample_airbnb
db.listingsAndReviews. ...
```


## C'est parti pour les exercices :


1. Find the price per night of the first record in the listingsAndReviews collection.
<details>
  <summary>Answer</summary>
  
	db.listingsAndReviews.findOne().price
	
</details>

2. Retrieve the cleaning fee of the first record in the listingsAndReviews collection.
<details>
  <summary>Answer</summary>
	
	db.listingsAndReviews.findOne().cleaning_fee
 
</details>

3. Find the host_name, host_location, host_about of the first record in the listingsAndReviews collection.
<details>
  <summary>Answer</summary>
  
	db.listingsAndReviews.find().limit(1).forEach(function(doc) {
		print("Host Name: " + doc.host.host_name);
		print("Host Location: " + doc.host.host_location);
		print("Host About: " + doc.host.host_about);
	});
	
</details>

4. Retrieve the number of bedrooms in the first record in the listingsAndReviews collection.
<details>
  <summary>Answer</summary>
  	
	db.listingsAndReviews.findOne().bedrooms
	
</details>

5. Retrieve the number of guests are included in the first record in the listingsAndReviews collection.
<details>
  <summary>Answer</summary>
  
	db.listingsAndReviews.findOne().guests_included

</details>

6. Write a MongoDB query to check whether the host have a profile picture in the first record in the listingsAndReviews collection.
<details>
  <summary>Answer</summary>
  
	db.listingsAndReviews.findOne({}, {"host.host_has_profile_pic": 1})

</details>

7. Write a MongoDB query to check whether the host's identity have been verified in the first record in the listingsAndReviews collection.
<details>
  <summary>Answer</summary>
  
	db.listingsAndReviews.findOne({}, {"host.host_identity_verified": 1})

</details>

8. Write a MongoDB query to find how many listings does the host have in the first records in the listingsAndReviews collection.

_Hint: use `aggregate`_

<details>
  <summary>Answer</summary>
  
	db.listingsAndReviews.aggregate([
		{ $match: {} },
		{ $project: { host_listings_count: "$host.host_listings_count" } },
		{ $limit: 1 }
	])

</details>

9. Write a MongoDB query to find the street address of the first record in the listingsAndReviews collection.
<details>
  <summary>Answer</summary>
  
	db.listingsAndReviews.findOne({}, {"address.street": 1})

</details>

10. Find all the listings in the listingsAndReviews collection where the property_type field is set to "House".
<details>
  <summary>Answer</summary>
  
	db.listingsAndReviews.find({ property_type: "House" })

</details>

11. Find all the listings in the listingsAndReviews collection with listing_url, name, host_name, host_location, reviewer_name and price that have a nightly price greater than $500.
<details>
  <summary>Answer</summary>


	db.listingsAndReviews.find(
	  {
		price: { $gt: 500 }
  	},
  	{
		listing_url: 1,
		name: 1,
    			"host.host_name": 1,
    			"host.host_location": 1,
    			"reviews.reviewer_name": 1,
		price: 1,
    		_id: 0
  	}
	)


</details>

12. Find all the listings in the listingsAndReviews collection that are located in Brazil and have a review score rating of at least 9. Return name, address, and review_scores_rating.
<details>
  <summary>Answer</summary>
  
	db.listingsAndReviews.find(
    	{
        	"address.country": "Brazil",
        	"review_scores.review_scores_rating": { $gte: 9 }
    	},
    	{
        	"name": 1,
        	"address": 1,
        	"review_scores.review_scores_rating": 1,
        	"_id": 0
    	}
    	)

</details>


13. Find all the listings with name, address, reviewer_name, and review_scores_rating in the listingsAndReviews collection that have a "hot tub" amenity and are located in the United States.
<details>
  <summary>Answer</summary>

	db.listingsAndReviews.find(
    	{
        	"address.country": "United States",
        	"amenities": "Hot tub",
        	"name": { "$exists": true },
        	"address": { "$exists": true },
        	"reviews.reviewer_name": { "$exists": true },
        	"review_scores.review_scores_rating": { "$exists": true }
    	},
    	{
        	"name": 1,
        	"address": 1,
        	"reviews.reviewer_name": 1,
        	"review_scores.review_scores_rating": 1
    	}
	)
	

</details>


14. Find all the listings with name, amenities and price in the listingsAndReviews collection that have a "pool" amenity and a nightly price between $200 and $400.
<details>
  <summary>Answer</summary>
  
	db.listingsAndReviews.find({
		amenities: "Pool",
		price: {
    			$gte: 200,
    			$lte: 400
  		}
	},
	{
		name: 1,
		amenities: 1,
		price: 1,
  		_id: 0
	})


</details>


15. Find all the listings with name, amenities and address in the listingsAndReviews collection that have a "Washer" amenity and are located in either Canada or Mexico.
<details>
  <summary>Answer</summary>
  
	db.listingsAndReviews.find({
		amenities: "Washer",
  		$or: [
			{ "address.country": "Canada" },
			{ "address.country": "Mexico" }
  		]
	},
	{
		name: 1,
		amenities: 1,
  			"address.country": 1,
  			"address.street": 1,
  			"address.city": 1,
  		_id: 0
	})


</details>


16. Find the top 10 most reviewed listings with listing_url, name, country, review_scoresin the listingsAndReviews collection.
<details>
  <summary>Answer</summary>
  
	db.listingsAndReviews.find({
	amenities: "Washer",
  	$or: [
		{ "address.country": "Canada" },
		{ "address.country": "Mexico" }
  	]
	},
	{
		name: 1,
		amenities: 1,
  			"address.country": 1,
  			"address.street": 1,
  			"address.city": 1,
  		_id: 0
	})


</details>


17. Find all the listings with listing_url, name, address and review_scores in the listingsAndReviews collection that have a "fireplace" amenity and a review score rating of at least 8.
<details>
  <summary>Answer</summary>
  
	db.listingsAndReviews.find({
  		"amenities": "Essentials",
  		"review_scores.review_scores_rating": { $gte: 8 }
	}, {
  		"listing_url": 1,
  		"name": 1,
  		"address": 1,
  		"review_scores": 1,
  		"_id": 0
	})


</details>


18. Find all the listings with listing_url, name, address and amenities, review scores in the listingsAndReviews collection that have a "washer" amenity and are located in either Italy or Spain.
<details>
  <summary>Answer</summary>
  
	db.listingsAndReviews.find({
  		"amenities": "Washer",
  	"$or": [
    		{"address.country": "Italy"},
    		{"address.country": "Spain"}
  		]
	}, {
  		"listing_url": 1,
  		"name": 1,
  		"address": 1,
  		"review_scores": 1,
  		"_id": 0
	})

</details>


19. Find the listings with listing_url, name, address and amenities, price, review scores in the listingsAndReviews collection that have the highest nightly prices.
<details>
  <summary>Answer</summary>
  	
	db.listingsAndReviews.find(
		{ "price": { "$exists": true } },
		{ "listing_url": 1, "name": 1, "address": 1, "amenities": 1, "price": 1, "review_scores": 1 }
	).sort({ "price": -1 }).limit(1)

</details>


20. Find the listings with listing_url, name, address and amenities,price,review scores in the listingsAndReviews collection that have the lowest nightly prices.
<details>
  <summary>Answer</summary>
  
	db.listingsAndReviews.find(
		{ "price": { "$exists": true } },
		{ "listing_url": 1, "name": 1, "address": 1, "amenities": 1, "price": 1, "review_scores": 1 }
	).sort({ "price": 1 }).limit(1)


</details>


21. Retrieve all documents with name, address, reviewer_name, review_scores_ratingin the listingsAndReviewscollection that have a number_of_reviews field is equal to 0.
<details>
  <summary>Answer</summary>

	db.listingsAndReviews.find(
    	{
		number_of_reviews: 0
    	},
    	{
		name: 1,
		address: 1,
		reviewer_name: 1,
		review_scores_rating: 1,
        	_id: 0
    	}
	)

</details>


22. Retrieve all documents with name, address, host, reviewer_name, review_scores_ratingin the listingsAndReviews collection where the host_is_superhost field is equal to true.
<details>
  <summary>Answer</summary>

   	db.listingsAndReviews.find(
		{ "host.host_is_superhost": true },
		{ name: 1, address: 1, "host.host_id": 1, "host.host_name": 1, reviewer_name: 1, review_scores_rating: 1 }
	)


</details>


23. Retrieve all documents with name, address, host, reviewer_name, review_scores_ratingin the listingsAndReviews collection where the coordinates field is not null.
<details>
  <summary>Answer</summary>

  	db.listingsAndReviews.find(
		{ "address.location.coordinates": { $ne: null } },
		{ "name": 1, "address": 1, "host": 1, "reviews.reviewer_name": 1, "review_scores.review_scores_rating": 1 }
	)

</details>


24. Retrieve all documents with name, address, host, bed_type, bed, review_scores_ratingfrom the listingsAndReviewscollection where the beds field is greater than or equal to 2.
<details>
  <summary>Answer</summary>

   	db.listingsAndReviews.find(
		{ "beds": { $gte: 2 } },
		{ "name": 1, "address": 1, "host": 1, "bed_type": 1, "beds": 1, "review_scores.review_scores_rating": 1 }
	)

</details>


25. Find all listings with name, address, host in the listingsAndReviews collection that have a host with a host_name containing the word "Livia".
<details>
  <summary>Answer</summary>

  	db.listingsAndReviews.find(
		{ "host.host_name": { $regex: "Livia", $options: "i" } },
		{ name: 1, address: 1, host: 1 }
	)

</details>


26. Find all listings with name, address, host in the listingsAndReviews collection that have a host with a host_location of "Brazil".
<details>
  <summary>Answer</summary>

  	db.listingsAndReviews.find(
		{ "host.host_location": "Brazil" },
		{ name: 1, address: 1, host: 1 }
	)

</details>

27. Retrieve all documents with name, address, host, availability in the listingsAndReviews collection where the availability_365 field is greater than 300.
<details>
  <summary>Answer</summary>

  	db.listingsAndReviews.find(
		{ "availability.availability_365": { $gt: 300 } },
		{ name: 1, address: 1, host: 1, availability: 1 }
	)

</details>


28. Retrieve all documents with listing_url, name, bedrooms, pricein the listingsAndReviews collectionwhere the bedrooms field is equal to 1.
<details>
  <summary>Answer</summary>

  	db.listingsAndReviews.find(
		{ bedrooms: 1 },
		{ listing_url: 1, name: 1, bedrooms: 1, price: 1 }
	)

</details>


29. Retrieve all documents with listing_url, name, bedrooms, cleaning_fee, and price in the listingsAndReviews collectionwhere the cleaning_fee field is not null.
<details>
  <summary>Answer</summary>

	db.listingsAndReviews.find(
		{ cleaning_fee: { $ne: null } },
		{ listing_url: 1, name: 1, bedrooms: 1, cleaning_fee:1,  price: 1 }
	)

</details>


30. Retrieve all documents with listing_url, name, bedrooms, pricein the listingsAndReviews collection where the price field is between 600 and 900.
<details>
  <summary>Answer</summary>

  	db.listingsAndReviews.find(
		{ price: { $gte: 600, $lte: 900 } },
		{ listing_url: 1, name: 1, bedrooms: 1, price: 1 }
	)

</details>


31. Retrieve all documents with listing_url, name, host, price in the listingsAndReviews collection where the host_verifications array contains "email".
<details>
  <summary>Answer</summary>

  	db.listingsAndReviews.find({
  		"host.host_verifications": "email"
	}, {
		listing_url: 1,
		name: 1,
		host: 1,
		price: 1
	}
	)

</details>


32. Retrieve all documents with listing_url, name, amenity, host in the listingsAndReviews collection where the amenities array contains both "TV" and "Wifi".
<details>
  <summary>Answer</summary>

  	db.listingsAndReviews.find(
   	{ 
		amenities: { $all: ["TV", "Wifi"] }
   	},
   	{ 
		listing_url: 1,
		name: 1,
		amenities: 1,
		host: 1
   	}
	)

</details>


33. Find all listings with listing_url, name, amenities, host in the listingsAndReviewscollection that have a host with a Jumio verification and a about section.
<details>
  <summary>Answer</summary>

  	db.listingsAndReviews.find({
  		"host.host_verifications": "jumio",
  		"host.host_about": { $exists: true, $ne: "" }
	},
	{
  		"listing_url": 1,
  		"name": 1,
  		"amenities": 1,
  		"host": 1
	})

</details>


34. Retrieve all documents with listing_url, name, host, price in the listingsAndReviews collection where the host_total_listings_count field is greater than 1.
<details>
  <summary>Answer</summary>

  	db.listingsAndReviews.find({
		"host.host_total_listings_count": { $gt: 1 }
	},
	{
  		"listing_url": 1,
  		"name": 1,
  		"host": 1,
  		"price": 1
	})

</details>


35. Retrieve all documents with listing_url, name, property_type, bed, price in the listingsAndReviewscollectionwhere the property_type field is equal to "Apartment" and the beds field is greater than or equal to 2.
<details>
  <summary>Answer</summary>

  	db.listingsAndReviews.find({
  		"property_type": "Apartment",
		"beds": { $gte: 2 }
	},
	{
  		"listing_url": 1,
  		"name": 1,
  		"property_type": 1,
  		"beds": 1,
  		"price": 1
	})

</details>


36. Find all listings with listing_url, name, property_type, bed, bathrooms, price in the listingsAndReviews collection that have a minimum of 2 bathrooms.
<details>
  <summary>Answer</summary>

	db.listingsAndReviews.aggregate([
  	{
    		$match: {
      			bathrooms: { $gte: 2 }
    		}
  	},
  	{
    	$project: {
      		listing_url: 1,
      		name: 1,
      		property_type: 1,
      		bed: 1,
		bathrooms: 1,
      		price: 1
    	}
  	}
	])

</details>

37. Find all listings with listing_url, name, property_type, bed, price, guests_included in the listingsAndReviews collection that have a maximum of 5 guests included in the price.
<details>
  <summary>Answer</summary>

	db.listingsAndReviews.aggregate([
  	{
    		$match: {
      			guests_included: { $lte: 5 }
    		}
  	},
  	{
    		$project: {
      			listing_url: 1,
      			name: 1,
      			property_type: 1,
      			bed: 1,
      			price: 1,
      			guests_included: 1
    		}
  	}
	])

</details>


38. Find all listings with listing_url, name, property_type, bed, price, security_deposit in the listingsAndReviews collection that have a price greater than $500 and a security deposit of $1000 or more.
<details>
  <summary>Answer</summary>

	db.listingsAndReviews.aggregate([
  	{
    		$match: {
      			price: { $gt: 500 },
      			security_deposit: { $gte: 1000 }
    		}
  	},
  	{
    	$project: {
      		listing_url: 1,
      		name: 1,
      		property_type: 1,
      		bed: 1,
      		price: 1,
      		security_deposit: 1
    	}
  	}
	])

</details>


39. Find all listings with listing_url, name, property_type, bed, price, cancellation_policy in the listingsAndReviews collection that have a cancellation policy of "flexible".
<details>
  <summary>Answer</summary>

	db.listingsAndReviews.aggregate([
  	{
    		$match: {
      			cancellation_policy: "flexible"
    		}
  	},
  	{
    		$project: {
      			listing_url: 1,
      			name: 1,
      			property_type: 1,
      			bed: 1,
      			price: 1,
      			cancellation_policy: 1
    		}
  	}
	])

</details>


40. Find all listings with listing_url, name, property_type, bed_type, amenities, price in the listingsAndReviews collection that have a real bed as the bed type and a kitchen amenity.
<details>
  <summary>Answer</summary>

	db.listingsAndReviews.aggregate([
  	{
    		$match: {
      			bed_type: "Real Bed",
      			amenities: "Kitchen"
    		}
  	},
  	{
    		$project: {
      			listing_url: 1,
      			name: 1,
      			property_type: 1,
      			bed_type: 1,
      			amenities: 1,
      			price: 1
    		}
  	}
	])

</details>


41. Find all listings with listing_url, name, address, amenities in the listingsAndReviews collection that have a 24-hour check-in amenity and are located in Brazil.
<details>
  <summary>Answer</summary>

	db.listingsAndReviews.find(
	{
    		"address.country": "Brazil",
    		"amenities": "24-hour check-in"
  	},
  	{
    		"listing_url": 1,
    		"name": 1,
    		"address": 1,
    		"amenities": 1
  	}
	);
	
</details>

42. Find all listings with listing_url, name, address, reviews in the listingsAndReviews collection that have at least one review.
<details>
  <summary>Answer</summary>

	db.listingsAndReviews.find(
  	{
    		reviews: { $exists: true, $not: { $size: 0 } }
  	},
  	{
    		listing_url: 1,
    		name: 1,
    		address: 1,
    		reviews: 1
  	}
	);
 
</details>


43. Find the number of documents that have a blank medium picture url in the istingsAndReviews collection.
<details>
  <summary>Answer</summary>

	db.listingsAndReviews.count({ "images.medium_url": "" });
	
</details>


44. Find all listings with listing_url, name, address, availability_30 in the istingsAndReviews collection that have an availability of at least 30 days.
<details>
  <summary>Answer</summary>

	db.listingsAndReviews.find(
	{ "availability.availability_30": { $gte: 30 } }, 
	{ 
		listing_url: 1, 
		name: 1, 
		address: 1, 
		"availability.availability_30": 1 
	})
  
</details>


45. Find all listings with listing_url, name, address in the listingsAndReviews collection that have a suburb of "Lagoa".
<details>
  <summary>Answer</summary>

	db.listingsAndReviews.find(
		{ "address.suburb": "Lagoa" }, 
		{ listing_url: 1, name: 1, address: 1 }
	)
	
</details>


46. Find all listings with listing_url, name, address, host in the listingsAndReviews collection that have a host who is a superhost and has at least 2 listings.
<details>
  <summary>Answer</summary>

	db.listingsAndReviews.find(
	{
    		"host.host_is_superhost": true,
    		"host.host_total_listings_count": { $gte: 2 }
  	},
  	{
    		listing_url: 1,
    		name: 1,
    		address: 1,
    		host: 1
  	}
	)
 
</details>


47. Find all listings with listing_url, name, address, host in the listingsAndReviews collection that have a host who has a profile pic and has been identity verified.
<details>
  <summary>Answer</summary>

	db.listingsAndReviews.find(
  	{
    		"host.host_has_profile_pic": true,
    		"host.host_identity_verified": true
  	},
  	{
    		listing_url: 1,
    		name: 1,
    		address: 1,
    		host: 1
  	}
	)
	
</details>


48. Write a mongodb query to find the listing_url, name, address, host_verifications, and size of host_verification under the host subdocument in the listingsAndReviews collection.
<details>
  <summary>Answer</summary>

	db.listingsAndReviews.aggregate([
  	{
    		$project: {
      			"listing_url": 1,
      			"name": 1,
      			"address": 1,
      			"host_verifications": "$host.host_verifications",
      			"host_verifications_count": { $size: "$host.host_verifications" }
    		}
  	}
	])
  
</details>


49. Find all listings with listing_url, name, address, host_verificationand size of host_verification array in the listingsAndReviews collection that have a host with at least 3 verifications.
<details>
  <summary>Answer</summary>

	db.listingsAndReviews.aggregate([
  	{
    		$match: {
      			"host.host_verifications": { $exists: true },
      			$expr: { $gte: [{ $size: "$host.host_verifications" }, 3] }
    		}
  	},
  	{
    		$project: {
      			"listing_url": 1,
      			"name": 1,
      			"address": 1,
      			"host.host_verifications": 1,
      			"host_verifications_count": { $size: "$host.host_verifications" }
    		}
  	}
	])
	
</details>


50. Find all listings with listing_url, name, address, host_picture_url in the listingsAndReviews collection that have a host with a picture url.
<details>
  <summary>Answer</summary>

	db.listingsAndReviews.aggregate([ 
	{ 
		$match: { 
			"host.host_picture_url": { $exists: true, $ne: null }
 		} 
	}, 
	{ 
 		$project: { 
			listing_url: 1, 
			name: 1, 
			address: 1, "host.host_picture_url": 1 
		} 
	}
	])
 
</details>


## Sources

- Documentation de MongoDB : https://www.mongodb.com/docs/manual/tutorial/query-documents/
- Query documents : https://www.mongodb.com/docs/manual/tutorial/query-documents/
- Query operators : https://www.mongodb.com/docs/manual/reference/operator/query/

