# machine_learning
contains machine learning codes &amp; documents

package ptit.simulate;
 
import java.util.Dictionary;
import java.util.HashMap;
import java.util.Hashtable;
import java.util.Random;
import java.util.RandomAccess;
 
public class simulate {
   
    int fault_type_num=5;
   
    double[][] perf_value = new double[5][3];//3 cot 5 hang, min | current | max
    double[] iterator_value = new double[5];//chua buoc nhay cua tung gia tri
   
    DataColector datacolector = new DataColector();
    Random Rand = new Random();
   
    public void init(){
       
       
        perf_value[0][0]=0.3;//min cpu
        perf_value[0][1]=0.3;//current cpu
        perf_value[0][2]=0.9;//max cpu
        iterator_value[0]=0.06;
       
        perf_value[1][0]=0.3;//min ram
        perf_value[1][1]=0.4;//current ram
        perf_value[1][2]=0.9;//max ram
        iterator_value[1]=0.07;
       
        perf_value[2][0]=0.02;//min respone
        perf_value[2][1]=0.02;//current respone
        perf_value[2][2]=0.2;//max respone
        iterator_value[2]=0.02;
       
        perf_value[3][0]=0.012;//min DiskLatency
        perf_value[3][1]=0.012;//current DiskLatency
        perf_value[3][2]=0.05;//max DiskLatency
        iterator_value[3]=0.014;
       
        perf_value[4][0]=0.45;//min NetworkUti
        perf_value[4][1]=0.45;//current NetworkUti
        perf_value[4][2]=0.8;//max NetworkUti
        iterator_value[4]=0.12;
       
    }
   
public boolean xacSuatLoi(){
    Random r = new Random();
    double xacsuat = r.nextDouble();
    if(xacsuat <= 0.2)
        return true;
    else return false;
}
 
public int chonLoi(){
    Random r = new Random();
    double xacsuat = r.nextDouble();
    //xac dinh loai loi
    for(int i = 1;i<= fault_type_num;i++){
        try {
            Thread.sleep(2);
        } catch (InterruptedException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        increasePerformanceValue();
        if( xacsuat < (double) (i * 1.0/fault_type_num))
            return i;//return loi thu i
    }
    return fault_type_num;
}
 
//tang performance cpu,ram... len tung buoc theo doi thi sin
public void increasePerformanceValue(){
    for(int i = 0; i< iterator_value.length; i++){
        perf_value[i][1] = roundResult(i);
    }
}
 
public simulate(){}
 
//lam ket qua cua current value luon trong nguong min va max
private double roundResult(int index){
    //double value= perf_value[index][1] + iterator_value[index];
    //Random r = new Random();
    if(perf_value[index][1] >= perf_value[index][2])// neu current > max, reset ve min
        return perf_value[index][0]; // tra ve gia tri min
    //long range = Math.round(iterator_value[index]*1000);
    //double step = (double) Rand.nextInt((int) range)/1000.0;
    double value= perf_value[index][1] + iterator_value[index] ;
    // neu value lon hon max return ve max, nguoc lai la value
    return value > perf_value[index][2]? perf_value[index][2] : value;
}
 
public void DoSimulate(int numOfResult){
    init();
    int count=0;
    int faultcode=0;
    datacolector.init();
    while(count < numOfResult){
        if(xacSuatLoi()){
            faultcode = chonLoi();
            datacolector.WriteData(perf_value[0][1], perf_value[1][1], perf_value[2][1], perf_value[3][1], perf_value[4][1], faultcode);
            count++;
        }
    }
    datacolector.close();
    //goi vaildate data
    //Validate();
}
 
    public void Validate(){
        datacolector.ValidateData();
    }
 
    public static void main(String[] args){
       
        simulate sim = new simulate();
        sim.DoSimulate(1000);
        sim.Validate();
        //for(int i =0;i<100;i++){
        //  System.out.println("abc");
        //}
    }
}
