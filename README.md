善融删除所有提醒代码更新 发表于 2020-12-13 13:13

function deleteAll(){
        jQuery.ajax({
                type : "POST",
                dataType : "json",
                timeout : 60000,
                url : "/commonData/getUserMyPayments.jhtml",
                data : {
                        cloudId:cloudId,
                        channel:channel
                },
                async:false,
                contentType : "application/x-www-form-urlencoded; charset=utf-8",
                success : function(result) {
                        console.log(result);
                       
                        for(var i =0;i<result.data.allMyPaymentListByGroup.length;i++){
                                                                       deleteRmnd(result.data.allMyPaymentListByGroup[i]);
                                        num++;
                        }
                        if(result.data.allMyPaymentListByGroup.length==30){
                                setTimeout(deleteAll,500);
                        }else{
                                alert("成功删除"+num+"个");

                        }
                }
        });
}
function deleteRmnd(cell){
        if(cell.isRmnd=="0"){
                deletePayment(cell.myPaymentId)
                return;
        }
        jQuery.ajax({
                type : "POST",
                dataType : "json",
                url : "/commonData/updatePaymentItem.jhtml",
                data : {
                        myPaymentId:cell.myPaymentId,
                        cloudId:cloudId,
                        urbnAtnmsdstcCd:"110000",
                        provAtnmsrgonCd:"110000",
                        projectMergeRegionCode:"110000",
                        billItem:cell.billItem,
                        billItemName:cell.projectName,
                        etruntId:cell.etruntId,
                        enruntNm:cell.enruntNm,
                        allLoginInfo:cell.allLoginInfo,
                        groupId:cell.groupId,
                        groupName:cell.groupName,
                        groupType:"1",
                        isRmnd:"0",
                        eventPeriod:"",
                        startDt:"",
                        endDt:"",
                        projectSource:cell.projectSource,
                        branNo:"110000000",
                        channel:'0'+ channel,
                        isDisplayRmnd:"false",
                        remindType:"",
                        rmndMobileInput:"",
                        isFamilyFlag:"no"
                },
                contentType : "application/x-www-form-urlencoded; charset=utf-8",
                success : function(data) {
                        console.log(data);
                        if(data.code == "success"){

                                deletePayment(cell.myPaymentId)
                        }
                }
        });
}
function deletePayment(myPaymentId){
        jQuery.ajax({
                type : "POST",
                dataType : "json",
                url : "/commonData/deleteMyPayment.jhtml",
                data : {
                        myPaymentId:myPaymentId,
cloudId:cloudId,
                        channel:channel
                },
                contentType : "application/x-www-form-urlencoded; charset=utf-8",
                success : function(data) {
                        console.log(data);
                        if(data.code == "success"){
                                console.log("删除成功");
                        }
                }
        });
}
var cloudId = $("#cloudId").val();
var channel = $("#channel").val();
var num=0;
deleteAll();
