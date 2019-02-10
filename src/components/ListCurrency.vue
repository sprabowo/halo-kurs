<template>
	<section class="list-currency">
		<div class="row" v-for="list in dataList" :key="list">
			<div class="list-currency__content">
				<div>1 USD = {{ __cal(dataInput, dataAmount[list])|thousand }} {{ list }} <small>({{ acronym[list] }})</small></div>
			</div>
			<div class="list-currency__navigation"><button @click="$emit('delete-list', list)" class="list-currency__button"> - </button></div>
		</div>
		<button v-if="showAdd" @click="showAdd = false" class="list-currency__addbutton">Add Currency</button>
		<div v-else class="row">
			<div class="list-currency__content">
				<select v-model="selectAdd" class="list-currency__select">
					<option disabled value="">SELECT YOUR CURRENCY</option>
					<option v-for="listCurr in dataListAll" :key="listCurr" :value="listCurr">{{ listCurr }}</option>
				</select>
			</div>
			<div class="list-currency__navigation"><button @click="__clickAdd" class="list-currency__button_left"> + </button></div>
		</div>
	</section>
</template>

<script>
/* eslint-disable */
export default {
	data () {
		return {
			showAdd: true,
			selectAdd: "",
			acronym: {     
				'EUR': 'Euro',
				'CAD': 'Canadian Dollar',
				'IDR': 'Indonesian Rupiah',
				'GBP': 'Great Britain Pound',
				'CHF': 'Switzerland Franc',
				'SGD': 'Singapore Dollar',
				'INR': 'India Rupee'
				,'MYR': 'Malaysia Ringgit',
				'JPY': 'Japan Yen',
				'KRW': 'South Korea Won'
			}
		}
	},
	props: ['dataList', 'dataListAll', 'dataAmount', 'dataInput'],
	filters: {
		thousand(val) {
			let bilangan = val || 0
			let	number_string = bilangan.toString()
			let sisa 	= number_string.length % 3
			let rupiah 	= number_string.substr(0, sisa)
			let ribuan 	= number_string.substr(sisa).match(/\d{3}/g)
			let koma = number_string.match(/\.\d+/g)[0]

			if (ribuan) {
				let separator = sisa ? ',' : ''
				rupiah += separator + ribuan.join(',')
			}
			return rupiah + koma
		}
	},
	methods: {
		__clickAdd () {
			this.showAdd = !this.showAdd
			this.$emit('add-list', this.selectAdd)
			this.selectAdd = ""
		},
		__num (s) {
			return parseFloat(s) || 0.00
		},
		__cal (a, b) {
			return this.__num(a * b).toFixed(2)
		}
	}
}
/* eslint-disable */
</script>


<style lang="scss">
.list-currency {
    @media screen and (max-width: 767px) {
        padding: 16px
    }
    border: 1px solid #c0c0c0;
    border-radius: 7px;
    padding: 32px;
    text-align: left;
}
.list-currency__content {
    display: flex;
    width: 90%;
    margin-bottom: 16px;
}
.list-currency__addbutton {
    width: 100%;
    cursor: pointer;
    height: 28px;
    color: #4a4a4a;
    border: 1px solid #f0f0f0;
    background-color: #ffffff;
    border-radius: 7px;
}
.list-currency__select {
    width: 100%;
    cursor: pointer;
    height: 28px;
    color: #4a4a4a;
    border: 1px solid #f0f0f0;
    background-color: #ffffff;
    border-radius: 7px 0px 0px 7px;
    border-right: none;
}
.list-currency__button {
    height: 28px;
    width: 28px;
    color: #4a4a4a;
    border: 1px solid #f0f0f0;
    background-color: #ffffff;
    border-radius: 7px;
}
.list-currency__button_left {
    height: 28px;
    width: 28px;
    color: #4a4a4a;
    border: 1px solid #f0f0f0;
    background-color: #ffffff;
    border-radius: 0px 7px 7px 0px;
}
.list-currency__navigation {
    width: 10%;
}
</style>

