<template>
	<div id="app">
		<ul class="navigation">
			<li>Template VueJS</li>
			<li>Disco</li>
		</ul>
		<h3><span style="text-decoration: underline; text-transform: uppercase">Artistes référencés</span></h3>
		<div class="container">
			<div class="recordsList">
				<div class="courses-container">
					<div v-for="(record, index) in records" :key="index"
						:class="[(record.pitchforkPos <= 10) ? topAlbum : '', 'card']">
						<div class="album-preview">
							<img height="100px" :src="hasCover(index)" alt="" />
						</div>
						<div class="course-info">
							<div class="stock-container">
								<span class="stock-text">
									{{ record.stock }} in Stock
								</span>
							</div>
							<h6><span v-if="record.pitchforkPos <= 10">👍 </span>{{ record.artist }}</h6>
							<h2>{{ record.title }} ( {{record.year}} )</h2>
							<h6 style="color: red" v-if="OutOfStock(index)">out of stock</h6>
							<button class="btn" @click="incrStock(index)">
								[+]
							</button>
							<button class="btn" @click="decrStock(index)">
								[-]
							</button>
						</div>
					</div>
				</div>
			</div>
			<div class="infos">
				<span style="text-transform: uppercase">
					Nombres de CD en stock : {{ stockTotal() }}
				</span>
			</div>
		</div>
	</div>
</template>

<script>
import { records } from "./assets/js/allRecords"
console.log(records)

export default {
	name: "App",
	data: () => ({
		nbAlbums: 0,
		topAlbum: 'topAlbum',
		cover: "http://via.placeholder.com/300",
		records: records,
		albums: [
			{
				artist: "KIKESA",
				album: "Rubi",
				stock: 10,
				cover: "https://static.fnac-static.com/multimedia/Images/FR/NR/a0/10/d7/14094496/1507-1/tsp20220224171211/Rubi-Edition-Limitee.jpg",
			},
			{
				artist: "BILLY TALENT",
				album: "Billy Talent II",
				stock: 10,
				cover: "https://static.fnac-static.com/multimedia/Images/FR/NR/63/e3/c9/13230947/1540-1/tsp20210122145154/Billy-Talent-II.jpg",
			},
			{
				artist: "METALLICA",
				album: "Master of puppets",
				stock: 10,
				cover: "https://m.media-amazon.com/images/I/71SziOTzXrL._SL1425_.jpg",
			},
			{
				artist: "MOTORHEAD",
				album: "Motorhead",
				stock: 10,
				cover: "https://m.media-amazon.com/images/I/41HtnRyB2TL.jpg",
			},
			{
				artist: "IGIT",
				album: "Jouons",
				stock: 10,
				cover: "https://static.fnac-static.com/multimedia/Images/FR/NR/30/f4/83/8647728/1540-1/tsp20170224110358/Jouons.jpg",
			}
		],
	}),
	methods: {
		decrStock(albumId) {
			if (this.records[albumId].stock >= 1) {
				this.records[albumId].stock--;
			}
		},
		incrStock(albumId) {
			this.records[albumId].stock++;
		},
		OutOfStock(albumId) {
			if (this.records[albumId].stock < 1) {
				return true
			}
			else {
				return false
			}
		},
		stockTotal() {
			let total = 0

			// Avec le reduce
			let totalReduce = this.records.reduce(
				(previousValue, currentValue) => previousValue + currentValue.stock, 0 
			);

			// Avec une boucle for
			for(let i = 0; i < this.records.length; i++) {
				total += this.records[i].stock
			}

			console.log(total)
			console.log(totalReduce)
			
			return totalReduce
		},
		hasCover(albumId) {
			return this.records[albumId]?.coverUrl ? this.records[albumId].coverUrl : this.cover
			
		}
	},
};
</script>

<style>
body {
	font-family: "Roboto", sans-serif;
	margin: 0;
}
.topAlbum {
	box-shadow: rgba(240, 46, 46, 0.4) 5px 5px, rgba(240, 46, 46, 0.3) 10px 10px, rgba(240, 46, 46, 0.2) 15px 15px, rgba(240, 46, 46, 0.1) 20px 20px, rgba(240, 46, 46, 0.05) 25px 25px !important;
}
.navigation {
	display: flex;
	flex-flow: row wrap;
	list-style: none;
	margin: 0;
	background: #2a265f;
	border-bottom: 1px solid grey;
}

.navigation li {
	text-decoration: none;
	display: block;
	padding: 1em;
	color: white;
	text-align: center;
}

h3 {
	padding: 1em;
}

.container {
	margin-top: 2rem;
	margin-left: 2rem;
	display: flex;
}

.recordsList {
	width: 820px;
}

.infos {
	width: 320px;
}

.card {
	background-color: #eeeef3;
	border-radius: 10px;
	box-shadow: 0 10px 10px rgba(0, 0, 0, 0.2);
	display: flex;
	max-width: 100%;
	overflow: hidden;
	margin-bottom: 16px;
	width: 700px;
}

.card h6 {
	opacity: 0.6;
	margin: 0;
	letter-spacing: 1px;
	text-transform: uppercase;
}

.card h2 {
	letter-spacing: 1px;
	margin: 10px 0;
}

.album-preview {
	background-color: #2a265f;
	color: #fff;
	padding: 20px;
	max-width: 250px;
}

.course-info {
	padding: 30px;
	position: relative;
	width: 100%;
}

.stock-container {
	position: absolute;
	top: 30px;
	right: 30px;
	text-align: right;
	width: 100px;
}

.stock-text {
	font-size: 12px;
	opacity: 0.6;
	letter-spacing: 1px;
}

.btn {
	background-color: #2a265f;
	border: 0;
	border-radius: 50px;
	box-shadow: 0 10px 10px rgba(0, 0, 0, 0.2);
	color: #fff;
	font-size: 12px;
	margin-top: 10px;
	padding: 6px 10px;
}

.infos {
	background-color: #e7efff;
	padding: 1rem;
	border-radius: 6px;
	box-shadow: 0 10px 10px rgba(0, 0, 0, 0.2);
}
</style>
