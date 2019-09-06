<style>
#game-view{
    height: 500px;
    width: 500px;
    border: 1px solid black;
}
</style>


<template>
    <div>
        <div v-for="(node, index) in nodes" :key="index">

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
                        flagged: false,
                        isBomb: false,
                        active: false
                    };

                    this.nodes.push(node);
                }
            }
        }
    }
}
</script>
