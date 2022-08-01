package jp.suntech.s21035.bmicalculators21035;

import androidx.appcompat.app.AppCompatActivity;
import androidx.fragment.app.DialogFragment;

import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;

import org.w3c.dom.Text;

public class MainActivity extends AppCompatActivity {
    private Button clear;
    private Button enterBMI;
    private Button enterBMI_2;
    private EditText age;//年齢
    private EditText input_H;//身長
    private EditText input_W;//体重
    private TextView bmi_result;
    private TextView msg;
    private TextView msg2;
    private TextView msg3;
    private TextView body_W;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        enterBMI = findViewById(R.id.button);
        clear = findViewById(R.id.button2);
        age = findViewById(R.id.et_age);
        input_H = findViewById(R.id.et_height);
        input_W = findViewById(R.id.et_body);
        bmi_result = findViewById(R.id.text_BMI_View);
        msg = findViewById(R.id.msg_1);
        msg2 = findViewById(R.id.msg_2);
        msg3 = findViewById(R.id.msg_3);
        body_W = findViewById(R.id.body);

        enterBMI.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                try {
                    //年齢比較
                    double input_age = Double.valueOf(age.getText().toString());

                    if (input_age < 16) {
                        MyDialog myDialog = new MyDialog();
                        myDialog.show(getSupportFragmentManager(), "MyDialog");
                    }

                    double input_H_double = Double.valueOf(input_H.getText().toString());
                    double input_W_double = Double.valueOf(input_W.getText().toString());
                    //適正体重計算
                    double BMI = enterBMI(input_H_double);
                    String result = String.format("%.1f", BMI);
                    body_W.setText(result);
                    //BMI計算
                    double BMI2 = enterBMI_2(input_H_double, input_W_double);
                    String result_BMI = String.format("%.1f", BMI2);

                    //テンプレメッセージ表示
                    String msg_1 = String.format("あなたの肥満度は");
                    String msg_2 = String.format("あなたの適正体重は");
                    String msg_3 = String.format("kg");

                    msg.setText(msg_1);
                    msg2.setText(msg_2);
                    msg3.setText(msg_3);

                    //結果メッセージ表示
                    String Good = String.format("適正体重");
                    String Over_4 = String.format("肥満度(4度)");
                    String Over_3 = String.format("肥満度(3度)");
                    String Over_2 = String.format("肥満度(2度)");
                    String Over_1 = String.format("肥満度(1度)");
                    String Normal = String.format("普通体重");
                    String Out = String.format("低体重");

                    if (BMI2 == 20) {
                        bmi_result.setText(Good);
                    } else if (BMI2 > 40) {
                        bmi_result.setText(Over_4);
                    } else if (BMI2 > 35 && BMI2 < 40) {
                        bmi_result.setText(Over_3);
                    } else if (BMI2 > 30 && BMI2 < 35) {
                        bmi_result.setText(Over_2);
                    } else if (BMI2 > 25 && BMI2 < 30) {
                        bmi_result.setText(Over_1);
                    } else if (BMI2 > 18.5 && BMI2 < 25) {
                        bmi_result.setText(Normal);
                    } else if (BMI2 < 18.5) {
                        bmi_result.setText(Out);
                    }

                    Log.d("MainActivity", "BMI=2" + BMI);
                }catch (NumberFormatException e){
                    System.out.println("空白です");
                }

            }
        });
        clear.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                age.setText("");
                input_H.setText("");
                input_W.setText("");
                msg.setText("");
                msg2.setText("");
                msg3.setText("");
                bmi_result.setText("");
                body_W.setText("");

            }
        });
    }

    //適正体重　計算
    private double enterBMI(double h){
        double bmi=0;
        h=h/100;
        bmi = (h*h)*22;
        return bmi;
    }

    //BMI計算
    private double enterBMI_2(double h,double w){
        double bmi=0;
        if(w>0 && h>0){
            bmi = w/(h*h)*10000;
        }
        return bmi;
    }
}