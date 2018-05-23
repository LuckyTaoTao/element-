# element-
使用Cascader 级联选择器，来进行三级联动服务器请求操作



初始化得时候
this.$axios({
        method: "get",
        url: this.$api.getAreaList,
        params: {level:2,is_page:1}
      }).then(res => {
           var province={}
        for(var i in res.data.data){
          var province={}
          province.label=res.data.data[i].area_name
          province.value=res.data.data[i].area_id
          province.cities=[]
          this.options2.push(province);
        }
    });   请求第一级操作
    
    
 当点击第一级操作以后
 
  handleItemChange(val){
      var p_id;
      if(val.length == 1){
         p_id = val[0];
      }
      if(val.length == 2){
         p_id = val[1]
      }
      this.$axios({
        method: "get",
        url: this.$api.getAreaList,
        params: {p_id:p_id,is_page:1}
      }).then(res => {
        for(var i in this.options2){
          if(val.length == 1 && this.options2[i].value == val[0] && !this.options2[i].cities.length){
            for( var j in res.data.data){
              var cities = {};
              cities.label=res.data.data[j].area_name
              cities.value=res.data.data[j].area_id
              cities.cities=[]

              this.options2[i].cities.push(cities);
            }

            
          }

          if(val.length == 2){
            for(var k in this.options2[i].cities){
              console.log(this.options2[i].cities[k]);
              if(this.options2[i].cities[k].value == val[1] && !this.options2[i].cities[k].cities.length){
                for( var j in res.data.data){
                  var cities = {};
                  cities.label=res.data.data[j].area_name
                  cities.value=res.data.data[j].area_id

                  this.options2[i].cities[k].cities.push(cities);
                }
              }
            }
            
          }
          
        }
      });
    }
      
