<style scoped>
#game-view{
    height: 500px;
    width: 500px;
    border: 1px solid black;
}

.tile{
    width: 8px;
    height: 8px;
    background-color: azure;
    border: 1px solid #c3c3c3;
}
</style>

<template>
    <div>
        <div class="tile" v-for="(node, index) in nodes" :key="index">
        </div>
    </div>
</template>

<script>
export default {
    name: 'mineSweeper',
    data(){
        return{
            gameView:{},
            ctx:{},
            gameBoard:{
                width: 10,
                height: 10
            },
            nodes:[]
        }
    },
    mounted: function(){
        this.createNodes(this.gameBoard.width, this.gameBoard.height);
    },
    methods:{
        createNodes(width, height){
            for(var x = 0; x < width; x ++){
                for(var y = 0; y < height; y++){
                    const node = {
                        x: x,
                        y: y,
                        bombCount: 0,
                        flagged: false,
                        isBomb: false,
                        active: false
                    };

                    this.nodes.push(node);
                }
            }

            this.generateBombs();
        },
        generateBombs(){
            const totalNodes = this.gameBoard.width * this.gameBoard.height;
            const numberOfBombs = 35;

            var array = Array.from(Array(totalNodes).keys());

            for(var i = 0; i < numberOfBombs; i++){
                const randomIndex = this.randomRange(0, array.length);
                
                this.nodes[array[randomIndex]].isBomb = true;
                array.splice(randomIndex, 1);

                console.log(`Bomb No: ${1} \nRandomIndex: ${randomIndex} \nIndex: ${array[randomIndex]}`);
            }
        },
        randomRange(min, max){
            min = Math.ceil(min);
            max = Math.floor(max);
            return Math.floor(Math.random() * (max - min + 1)) + min;
        }
    }
}
</script>
