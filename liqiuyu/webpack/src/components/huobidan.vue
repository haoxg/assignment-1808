<template>
    <div v-cloak>
        <table id="tab">
            <tr>
                <td>品名</td>
                <td>数量</td>
                <td>单价(元)</td>
                <td :rowspan="productList.length+1">
                    <table id="money">
                        <tr>
                            <td colspan="5" id="money_tit">金额</td>
                        </tr>
                        <tr id="unit">
                            <td>万</td>
                            <td>千</td>
                            <td>百</td>
                            <td>十</td>
                            <td>元</td>
                        </tr>
                        <tr :key="'s'+index" v-for="(amount,index) in amountList">
                            <td :key="'w'+ins" v-for="(a,ins) in amount">{{a}}</td>
                        </tr>
                    </table>
                </td>
                <td>备注</td>
            </tr>
            <tr :key="'t'+indexs" v-for="(product,indexs) in productList">
                <td><input type="text" v-model="product.name" style="text-align:center;" @focus="focus" @blur="blur"></td>
                <td><input type="text" v-model="product.number" style="text-align:center;" @focus="focus" @blur="blur"></td>
                <td><input type="text" v-model="product.price" style="text-align:center;" @focus="focus" @blur="blur"></td>
                <td><input type="text" v-model="product.beizhu" style="text-align:center;" @focus="focus" @blur="blur"></td>
            </tr>
            <tr>
                <td>合计人民币（大写）</td>
                <td colspan="4">{{total|chinese}}</td>
            </tr>
        </table>
    </div>
</template>

<script>
    export default {
         data(){
             return {
                productList: [{
                    name: 1,
                    number: '',
                    price: '',
                    beizhu: ''
                }, {
                    name: 1,
                    number: '',
                    price: '',
                    beizhu: ''
                }]
            }
         } ,
        filters: {
            chinese(value) {
                var list1 = ['', '壹', '贰', '叁', '肆', '伍', '陆', '柒', '捌', '玖'];
                var list2 = ['整元', '拾', '佰', '仟', '万', '拾', '佰', '仟', '亿', ];
                value += '';
                var a = '';
                for (var i = value.length; i > 0; i--) {
                    a += value.charAt(i - 1) == 0 ? '零' : list2[value.length - i]
                    a += list1[value.charAt(i - 1)];
                };
                return a.split("").reverse().join("");
            }
        },
        computed: {
            amountList() {
                var arr = [];
                this.productList.forEach(element => {
                    var a = element.price * element.number + '';
                    var b = 5 - a.length;
                    for (var i = 0; i < b; i++) {
                        a = ' ' + a;
                    }
                    arr.push(a);
                });
                return arr;
            },
            total() {
                var total = 0;
                this.productList.forEach(element => {
                    total += element.price * element.number;
                })
                return total;
            }
        },
        methods: {
            focus() {
                if (focus) event.target.style.textAlign = 'left';
            },
            blur() {
                if (blur) event.target.style.textAlign = 'center';
            }
        }
    }
</script>

<style scoped>
     * {
        margin: 0;
        padding: 0;
    }
    
    table,
    table td {
        border: 1px solid #ccc;
        border-collapse: collapse;
        text-align: center;
    }
    
    #tab td {
        min-width: 130px;
        height: 50px;
    }
    
    #money td {
        min-width: 25px;
        height: 50px;
    }
    
    #unit td {
        height: 25px;
    }
    
    #money #money_tit {
        height: 25px;
    }
    
    input {
        min-width: 130px;
        height: 100%;
    }
    
    [v-cloak] {
        display: none;
    }
</style>
