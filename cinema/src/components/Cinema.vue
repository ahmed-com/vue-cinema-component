<template>
    <div :class="`body ${inputClass}`">

        <ul class="showcase">
            <li>
                <div class="seat"></div>
                <small>N/A</small>
            </li>
            <li>
                <div class="seat selected"></div>
                <small>Selected</small>
            </li>
            <li>
                <div class="seat occupied"></div>
                <small>Occupied</small>
            </li>
        </ul>

        <div class="container" id="container">
            <div class="screen"></div>
            <div v-for="n in 6" :key="n" class="row">
                <div
                    v-for="m in 8" 
                    :key="m" 
                    class="seat" 
                    :class="seats[getIndex(n,m)]"
                    @click="seats[getIndex(n,m)] === 'occupied' ? 0 : selectSeat(getIndex(n,m))"
                ></div>
            </div>
        </div>
    </div>
</template>

<script>

export default {
    props:[
        'value',
        'classList'
    ],

    data: function () {
        return {
            seats: this.value
        }
    },

    computed:{
        inputClass(){
            let className =''
            this.classList?.forEach(cssClass => {
                className += `${cssClass} `;
            });
            return className.trim();
        }
    },

    methods:{
        getIndex(n,m){
            return (n-1)*8 + m - 1;
        },

        selectSeat(index){
            if(this.seats[index] === 'selected'){
                this.$set(this.seats,index,'');
            }else{
                this.$set(this.seats,index,'selected');
            }
            this.$forceUpdate();
            this.$emit('input',this.seats);
        }
    }
}

</script>

<style scoped>
    @import url('https://fonts.googleapis.com/css2?family=Lato:wght@100&display=swap');

    .body{
        background-color: #242333;
        display: flex;
        flex-direction: column;
        color: white;
        align-items: center;
        justify-content: center;
        height: 100vh;
        font-family: 'Lato', sans-serif;
        margin: 0;
    }

    .seat{
        background-color: #444451;
        height: 12px;
        width: 15px;
        margin: 3px;
        border-top-left-radius: 10px;
        border-top-right-radius: 10px;
    }

    .seat.selected{
        background-color: #6feaf6;
    }

    .seat.occupied{
        background-color: white;
    }

    .seat:nth-of-type(2){
        margin-right: 18px;
    }

    .seat:nth-last-of-type(2){
        margin-left: 18px;
    }

    .showcase .seat:not(.occupied):hover{
        cursor: default;
        transform: scale(1);
    }

    .showcase{
        background-color: rgba(0, 0, 0, 0.1);
        padding: 5px 10px;
        border-radius: 5px;
        color: #777;
        list-style-type: none;
        display: flex;
        justify-content: space-between;
    }

    .showcase li{
        display: flex;
        align-items: center;
        justify-content: center;
        margin: 0 10px;
    }

    .showcase li small{
        margin-left: 2px;
    }

    .screen{
        background-color: white;
        height: 70px;
        width: 100%;
        margin: 15px 0;
        transform: rotateX(-45deg);
        box-shadow: 0 5px 15px rgba(255, 255,255,0.7);
    }

    .seat:not(.occupied):hover{
        cursor: pointer;
        transform: scale(1.2);
    }

    .row {
        display: flex;
    }

    .container{
        perspective: 500px;
        margin-bottom: 30px;
    }

</style>