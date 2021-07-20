<details>
	<summary>Sample document</summary>
	
```bson
{
	"_id" : ObjectId("60f6ddee24e687c0ca03f581"),
	"id" : 82,
	"url" : "http://www.tvmaze.com/shows/82/game-of-thrones",
	"name" : "Game of Thrones",
	"type" : "Scripted",
	"language" : "English",
	"genres" : [
		"Drama",
		"Adventure",
		"Fantasy"
	],
	"status" : "Running",
	"runtime" : 60,
	"premiered" : "2011-04-17",
	"officialSite" : "http://www.hbo.com/game-of-thrones",
	"schedule" : {
		"time" : "21:00",
		"days" : [
			"Sunday"
		]
	},
	"rating" : {
		"average" : 9.4
	},
	"weight" : 99,
	"network" : {
		"id" : 8,
		"name" : "HBO",
		"country" : {
			"name" : "United States",
			"code" : "US",
			"timezone" : "America/New_York"
		}
	},
	"webChannel" : {
		"id" : 22,
		"name" : "HBO Go",
		"country" : {
			"name" : "United States",
			"code" : "US",
			"timezone" : "America/New_York"
		}
	},
	"externals" : {
		"tvrage" : 24493,
		"thetvdb" : 121361,
		"imdb" : "tt0944947"
	},
	"image" : {
		"medium" : "http://static.tvmaze.com/uploads/images/medium_portrait/143/359013.jpg",
		"original" : "http://static.tvmaze.com/uploads/images/original_untouched/143/359013.jpg"
	},
	"summary" : "<p>Based on the bestselling book series <i>A Song of Ice and Fire</i> by George R.R. Martin, this sprawling new HBO drama is set in a world where summers span decades and winters can last a lifetime. From the scheming south and the savage eastern lands, to the frozen north and ancient Wall that protects the realm from the mysterious darkness beyond, the powerful families of the Seven Kingdoms are locked in a battle for the Iron Throne. This is a story of duplicity and treachery, nobility and honor, conquest and triumph. In the <b>Game of Thrones</b>, you either win or you die.</p>",
	"updated" : 1532947493,
	"_links" : {
		"self" : {
			"href" : "http://api.tvmaze.com/shows/82"
		},
		"previousepisode" : {
			"href" : "http://api.tvmaze.com/episodes/1221415"
		}
	}
}
```
</details>

<details>
<summary>1. Shows with average rating more than 9.3</summary>
<p>

```console
> db.shows.find({"rating.average": {$gt: 9.3}})
```

</p>
</details>

<details>
<summary>2. Shows with language other than 'English'</summary>
<p>

```console
> db.shows.find({"language": {$ne: "English"}})
```

</p>
</details>

<details>
<summary>3. English animation shows.</summary>
<p>

```console
> db.shows.find({language: "English", type: "Animation" })
```

</p>
</details>

<details>
<summary>4. 'Horror' shows that plays on Sunday </summary>
<p>

```console
> db.shows.find({"schedule.days":"Sunday"},{name:1})
```

</p>
</details>

<details>
<summary>5. Distinct web-channels of all shows</summary>
<p>

```console
> db.shows.aggregate([{$group:{_id:"$webChannel.name"}}])
```

</p>
</details>

<details>
<summary>6. Distinct country codes</summary>
<p>

```console
> db.shows.aggregate([{$group:{_id:"$network.country.code"}}])
```

</p>
</details>

<details>
<summary>7. All running shows in US</summary>
<p>

```console
> db.shows.find({status: "Running", "network.country.code": "US"})
```

</p>
</details>

<details>
<summary>8. Thrillers @sunday 22:00 hrs with minimum rating 5. Show time, name and rating only.</summary>
<p>

```console
> db.shows.find({
	"rating.average": {$gte: 5},
	genres: "Thriller",
	"schedule.days": "Sunday",
	"schedule.time": {$gte: "22:00"}
}, {
	"schedule.time": 1,
	name: 1,
	"rating.average": 1
})
```

</p>
</details>

<details>
<summary>9. 2 most recent Japanese shows with maximum 1 hour runtime.</summary>
<p>

```console
> db.shows.find({language: "Japanese", runtime: {$lte: 60}}).sort({updated: -1}).limit(2)
```

</p>
</details>

<details>
<summary>10. Name of the past comedy shows with rating below 5.</summary>
<p>

```console
> db.shows.find({status: "Ended", "rating.average": {$lt: 5}, genres: "Comedy"}, {name: 1})
```

</p>
</details>
