1.Создаём новый проект

2. Добавляем на форму 2 компонента типа TextEdit – один для ввода телефонного номера, другой для ввода текста СМС-сообщения

3.Добавляем на форму 2 компонента типа Button – один для кнопки «Звонок», другой для кнопки «СМС»
![image](https://user-images.githubusercontent.com/38504787/145621945-b248daf0-e1b8-4220-beb7-968894a456e9.png)

4. Добавим разрешения на осуществления вызова и отправки СМС в файле AndroidManifest.xml
~~~ xml
<uses-permission android:name="android.permission.CALL_PHONE" />
<uses-permission android:name="android.permission.SEND_SMS" />
~~~
5. Прописываем обработчик события кнопки отправки сообщения
~~~ java
public void onSms(View view) {
        EditText edit_Number=(EditText)findViewById(R.id.editTextPhone);
        String phoneNo = edit_Number.getText().toString();
        EditText sms_edit=(EditText)findViewById(R.id.editTextTextMultiLine2);
        String toSms="smsto:"+edit_Number.getText().toString();
        String messageText= sms_edit.getText().toString();
        Intent sms=new Intent(Intent.ACTION_SENDTO, Uri.parse(toSms));
        sms.putExtra("sms_body", messageText);
        startActivity(sms);
        SmsManager.getDefault().sendTextMessage(phoneNo, null,
                messageText.toString(), null, null);
    }
~~~
6. Прописываем код в MainActivity
~~~ java
protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Button mDialButton = (Button) findViewById(R.id.button2);
        final EditText mPhoneNoEt = (EditText)
                findViewById(R.id.editTextPhone);
        final EditText smsEdit = (EditText) findViewById(R.id.editTextTextMultiLine2);
        mDialButton.setOnClickListener(new View.OnClickListener()
        {
            @Override
            public void onClick(View view)
            {
                String phoneNo = mPhoneNoEt.getText().toString();
                if(!TextUtils.isEmpty(phoneNo))
                {
                    String dial = "tel:" + phoneNo;
                    startActivity(new Intent(Intent.ACTION_CALL,
                            Uri.parse(dial)));
                }
                else {
                    Toast.makeText(MainActivity.this, "Введите номер телефона",
                            Toast.LENGTH_SHORT).show();
                }
            }
        });
    }
~~~
7. Проверяем

![image](https://user-images.githubusercontent.com/38504787/145622317-6a240d94-167a-4d5b-9889-920df5a657f0.png)
