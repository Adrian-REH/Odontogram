# Odontogram
Un antes y despues del uso de Kotlin con XML a usar Kotlin con Jetpack Compose y simplificando un poco mas el codigo de un Odontograma


## Antes
Vista
```kotlin
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@color/grey"
    tools:context=".OdontogramaActivity">


    <TextView
        android:layout_marginTop="5dp"
        android:layout_gravity="center"
        android:layout_centerHorizontal="true"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Odontograma"
        android:textSize="20dp"
        android:textColor="@color/black"
        />

    <ScrollView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_marginTop="40dp">

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:layout_marginHorizontal="20dp"
            android:gravity="center"
            android:orientation="vertical">


            <com.google.android.material.textfield.TextInputLayout
                style="@style/Widget.MaterialComponents.TextInputLayout.OutlinedBox.ExposedDropdownMenu"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginHorizontal="20dp"
                android:layout_marginTop="10dp"

                android:hint="Numero de diente">

                <AutoCompleteTextView
                    android:id="@+id/txteprestacion"
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:focusable="false"
                    android:gravity="center" />

            </com.google.android.material.textfield.TextInputLayout>

            <androidx.cardview.widget.CardView
                android:id="@+id/carddientedent"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_marginTop="20dp"
                android:onClick="dialog1"
                app:cardCornerRadius="100dp"
                app:cardElevation="5dp">

                <ImageView
                    android:id="@+id/dienteinf"
                    android:layout_width="120dp"
                    android:layout_height="280dp"
                    android:layout_margin="20dp" />
            </androidx.cardview.widget.CardView>

            <LinearLayout
                android:id="@+id/lindienteinf"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:orientation="vertical">

                <androidx.recyclerview.widget.RecyclerView
                    android:id="@+id/rcydienteinf"
                    android:layout_width="match_parent"
                    android:layout_height="330dp" />

            </LinearLayout>

            <androidx.cardview.widget.CardView
                android:id="@+id/carddientfuera"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_marginVertical="10dp"
                android:onClick="dialog2"
                app:cardCornerRadius="100dp"
                app:cardElevation="5dp">

                <ImageView
                    android:id="@+id/dientesup"
                    android:layout_width="130dp"
                    android:layout_height="130dp"
                    android:layout_margin="20dp" />
            </androidx.cardview.widget.CardView>

            <LinearLayout
                android:id="@+id/lindientesup"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:orientation="vertical">

                <androidx.recyclerview.widget.RecyclerView
                    android:id="@+id/rcydientesup"
                    android:layout_width="match_parent"
                    android:layout_height="180dp" />

            </LinearLayout>

            <ImageView
                android:id="@+id/viewimghelp"
                android:layout_width="wrap_content"
                android:layout_height="400dp"
                android:src="@drawable/odontograma" />

        </LinearLayout>


    </ScrollView>


    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_alignParentBottom="true"
        android:gravity="center"
        android:orientation="horizontal">

        <com.google.android.material.floatingactionbutton.FloatingActionButton
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginEnd="10dp"
            android:layout_marginVertical="10dp"
            android:backgroundTint="@color/white"
            android:baselineAlignBottom="true"
            android:src="@drawable/ic_save"
            app:backgroundTint="#00FFFFFF"
            android:onClick="Back"
            app:tint="@color/black">

        </com.google.android.material.floatingactionbutton.FloatingActionButton>

        <com.google.android.material.floatingactionbutton.FloatingActionButton
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginEnd="10dp"
            android:backgroundTint="@color/white"
            android:baselineAlignBottom="true"
            android:src="@drawable/ic_photo"
            app:backgroundTint="#00FFFFFF"
            app:tint="@color/black"></com.google.android.material.floatingactionbutton.FloatingActionButton>

        <com.google.android.material.floatingactionbutton.FloatingActionButton
            android:id="@+id/btnhelp"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:backgroundTint="@color/white"
            android:baselineAlignBottom="true"
            android:onClick="help"
            android:src="@drawable/ic_help"
            app:backgroundTint="#00FFFFFF"
            app:tint="@color/black"></com.google.android.material.floatingactionbutton.FloatingActionButton>

    </LinearLayout>


</RelativeLayout>
```
Funciones
```kotlin

class OdontogramaActivity : AppCompatActivity(),AdapterDientInfError.onDientInfItemClick,AdapterDientSupError.onDientSupItemClick,AdapterView.OnItemClickListener {

    var name:String?=""
    var url:String?=""
    var IDP:String?=""
    var dni:String?=""
    var Ndient:Int=0
    var fecha:Int=0
    var JSONDiente:String?=""
    var JSONDienteT:String?=""
    var Diente:String?=""
    var id:Int=0

    var codigo:Int=0
    var imghelp:Boolean=true

    var JSONODONTOGRAMA: JSONObject? =null
    var JSONArrayODONTOGRAMA: JSONArray? =null

    val arraylistOdnt= ArrayList<Odontograma>()
    val arraylisError= ArrayList<Odontograma>()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_odontograma)
        lindienteinf.visibility=View.GONE
        viewimghelp.visibility=View.GONE

        for (j in 1 until 15) {
            arraylisError.add(Odontograma("http://$url/docs/dientes/inf$j.png","http://$url/docs/dientes/sup$j.png"))

        }

        val arrayList32=ArrayList<String>()

        for (i in 0 until 32) {
            arrayList32.add("${i+1}")
        }

        if(intent.extras !=null){
            dni = intent.getStringExtra("dni")
            //correo = intent.getStringExtra("correo")
            IDP = intent.getStringExtra("IDP")
            url = intent.getStringExtra("url")
            name = intent.getStringExtra("name")
            fecha = intent.getStringExtra("fecha").toString().toInt()
            id = intent.getStringExtra("id").toString().toInt()
            codigo = intent.getStringExtra("codigo")?.toInt() ?: 0

        }


        try {
            if (codigo==0){
                for (j in 0 until 33) {

                    var Diente= ("{\n" +
                            " \"d${j+1}\": ${"{\n" +
                                    " \"url1\": \"http://23herrera.xyz:81/appointment/docs/dientes/inf${j+1}.png\",\n" +
                                    " \"url2\": \"http://23herrera.xyz:81/appointment/docs/dientes/sup${j+1}.png\"\n" +
                                    " }"}\n" +
                            " }")

                    if (JSONDiente==""){
                        JSONDiente = Diente
                    }else if (JSONDiente!=""){
                        JSONDiente = JSONDiente+","+Diente
                    }

                }
                JSONDienteT="{\"data\":[$JSONDiente]}"
                JSONODONTOGRAMA= JSONObject(JSONDienteT)
                JSONArrayODONTOGRAMA= JSONODONTOGRAMA!!.getJSONArray("data")

            }
            else if (codigo!=0){
                getOdontogramaDatos(View(applicationContext),false)

            }
        }catch (e:Exception){
            Toast.makeText(this,"$e",Toast.LENGTH_LONG).show()
        }


        val arrayAdapter= ArrayAdapter(this,R.layout.dropdown_item,arrayList32)
        with(txteprestacion){
            setAdapter(arrayAdapter)
            onItemClickListener = this@OdontogramaActivity

        }
    }

    //RETROFIT INSTANCE
    fun getOdontogramaDatos(view: View,back:Boolean){

        CoroutineScope(Dispatchers.IO).launch {
        try {
            val call=RetrofitClient.instance.getOdontograma(dni.toString(),IDP.toString(),fecha.toString())
            val datos: OdontogramaRespons? =call.body()
            runOnUiThread{
                if(call.isSuccessful){
                    Toast.makeText(applicationContext,"Se encontro un odontograma",Toast.LENGTH_LONG).show()
                    if(datos != null){
                        if(codigo==datos.ID){

                            JSONDienteT="{\"data\":[${datos.dientes}]}"
                            JSONODONTOGRAMA= JSONObject(JSONDienteT)
                            JSONArrayODONTOGRAMA= JSONODONTOGRAMA!!.getJSONArray("data")

                            for (j in 0 until 32) {
                                val jsonObject= JSONArrayODONTOGRAMA!!.getJSONObject(j)
                                val JsonOdont=JSONObject(jsonObject?.getString("d${j+1}").toString())
                                arraylistOdnt.add(Odontograma(JsonOdont.getString("url1").toString(),JsonOdont.getString("url2").toString()))
                            }



                        }
                    }
                }
                else{
                    Toast.makeText(applicationContext,"ODONTOGRAMA no encontrado",Toast.LENGTH_LONG).show()
                }
            }

        }catch (e:Exception){

        }

        }
    }

    fun getOdontogramaSave(view: View,back: Boolean){

        CoroutineScope(Dispatchers.IO).launch {
            try{
                val call=RetrofitClient.instance.getOdontograma(dni.toString(),IDP.toString(),fecha.toString())
                val datos: OdontogramaRespons? =call.body()
                runOnUiThread{
                if (datos != null) {
                    if(call.isSuccessful){

                        if (codigo==0){
                            Toast.makeText(applicationContext,"Se encontro un Odontograma pero necesitas uno nuevo",Toast.LENGTH_LONG).show()
                            postOdontograma("insertar",back)
                        }else{
                            Toast.makeText(applicationContext,"Se encontro un Odontograma",Toast.LENGTH_LONG).show()
                            postOdontograma("modificar",back)
                        }
                    }else{
                        Toast.makeText(applicationContext,"No se encontro un odontograma o hay un error del srv",Toast.LENGTH_LONG).show()
                       // postOdontograma("insertar",back)
                    }
                }
                }
            }catch (e:Exception){
                if (codigo==0){
                    postOdontograma("insertar",back)
                }
            }
        }
    }

    private fun postOdontograma(accion:String,back: Boolean) {
        JSONDiente=""

        for (j in 0 until JSONArrayODONTOGRAMA!!.length()) {
                val jsonObject= JSONArrayODONTOGRAMA!!.getJSONObject(j)
                val JsonOdont=JSONObject(jsonObject?.getString("d${j+1}").toString())
                if(Ndient-1== j){
                    if (JSONDiente==""){
                        JSONDiente = Diente
                    }else if (JSONDiente!=""){
                        JSONDiente = JSONDiente+","+Diente
                    }

                }else
                {
                    var Diente= ("{\n" +
                            " \"d${j+1}\": ${"{\n" +
                                    " \"url1\": \"${JsonOdont.getString("url1")}\",\n" +
                                    " \"url2\": \"${JsonOdont.getString("url2")}\"\n" +
                                    " }"}\n" +
                            " }")
                    if (JSONDiente==""){
                        JSONDiente = Diente
                    }else if (JSONDiente!=""){
                        JSONDiente = JSONDiente+","+Diente
                    }
                }
            }







        CoroutineScope(Dispatchers.IO).launch {
            val call=RetrofitClient.instance.postOdontograma(JSONDiente.toString(),codigo.toInt(),"${IDP}","${dni}",fecha.toString(),accion,accion)
            call.enqueue(object : Callback<OdontogramaRespons> {
                override fun onFailure(call: Call<OdontogramaRespons>, t: Throwable) {
                    Toast.makeText(applicationContext,t.message,Toast.LENGTH_LONG).show()
                }
                override fun onResponse(call: Call<OdontogramaRespons>, response: retrofit2.Response<OdontogramaRespons>) {
                    Toast.makeText(applicationContext,"Odontograma guardado",Toast.LENGTH_LONG).show()
                    codigo=response.body()!!.ID
                    if (back){
                        val intent = Intent(applicationContext, TurnoActivity::class.java)
                        intent.putExtra("url",url)
                        intent.putExtra("IDP",IDP)
                        intent.putExtra("dni",dni)
                        intent.putExtra("name",name)
                        intent.putExtra("fecha",fecha.toString())
                        intent.putExtra("id",id.toString())
                        intent.putExtra("codigo",response.body()!!.ID.toString())
                        startActivity(intent)
                        finish()

                    }



                }
            })



        }
    }



    //DIALOGS
    fun dialog2(view: View){
        if (Ndient!=0){
            arraylisError.clear()

            btnhelp.visibility=View.GONE
            carddientfuera.visibility=View.GONE
            lindientesup.visibility=View.VISIBLE
            arraylisError.clear()
            arraylisError.add(Odontograma("https://cdn-icons-png.flaticon.com/512/91/91160.png","https://cdn-icons-png.flaticon.com/512/91/91160.png"))
            arraylisError.add(Odontograma("https://cdn-icons-png.flaticon.com/512/91/91160.png","https://cdn-icons-png.flaticon.com/512/91/91160.png"))
            arraylisError.add(Odontograma("https://cdn-icons-png.flaticon.com/512/91/91160.png","https://cdn-icons-png.flaticon.com/512/91/91160.png"))
            arraylisError.add(Odontograma("https://cdn-icons-png.flaticon.com/512/91/91160.png","https://cdn-icons-png.flaticon.com/512/91/91160.png"))
            arraylisError.add(Odontograma("https://cdn-icons-png.flaticon.com/512/91/91160.png","https://cdn-icons-png.flaticon.com/512/91/91160.png"))
            arraylisError.add(Odontograma("https://cdn-icons-png.flaticon.com/512/91/91160.png","https://cdn-icons-png.flaticon.com/512/91/91160.png"))
            arraylisError.add(Odontograma("https://cdn-icons-png.flaticon.com/512/91/91160.png","https://cdn-icons-png.flaticon.com/512/91/91160.png"))
            arraylisError.add(Odontograma("https://cdn-icons-png.flaticon.com/512/91/91160.png","https://cdn-icons-png.flaticon.com/512/91/91160.png"))
            arraylisError.add(Odontograma("https://cdn-icons-png.flaticon.com/512/91/91160.png","https://cdn-icons-png.flaticon.com/512/91/91160.png"))
            arraylisError.add(Odontograma("https://cdn-icons-png.flaticon.com/512/91/91160.png","https://cdn-icons-png.flaticon.com/512/91/91160.png"))
            arraylisError.add(Odontograma("https://cdn-icons-png.flaticon.com/512/91/91160.png","https://cdn-icons-png.flaticon.com/512/91/91160.png"))
            arraylisError.add(Odontograma("https://cdn-icons-png.flaticon.com/512/91/91160.png","https://cdn-icons-png.flaticon.com/512/91/91160.png"))
            arraylisError.add(Odontograma("https://cdn-icons-png.flaticon.com/512/91/91160.png","https://cdn-icons-png.flaticon.com/512/91/91160.png"))
            arraylisError.add(Odontograma("https://cdn-icons-png.flaticon.com/512/91/91160.png","https://cdn-icons-png.flaticon.com/512/91/91160.png"))
            arraylisError.add(Odontograma("https://cdn-icons-png.flaticon.com/512/91/91160.png","https://cdn-icons-png.flaticon.com/512/91/91160.png"))

            val adaptercliente = AdapterDientSupError(arraylisError, this, this)
            rcydientesup?.layoutManager = LinearLayoutManager(this, LinearLayoutManager.HORIZONTAL, false)

            rcydientesup?.adapter = adaptercliente
        }else{
            Toast.makeText(this,"Por favor selecciona un numero de Diente",Toast.LENGTH_SHORT).show()

        }

    }

    fun dialog1(view: View){
        if (Ndient!=0){
            arraylisError.clear()
            btnhelp.visibility=View.GONE
            carddientedent.visibility=View.GONE
            lindienteinf.visibility=View.VISIBLE
            arraylisError.clear()
            arraylisError.add(Odontograma("https://cdn-icons-png.flaticon.com/512/91/91160.png","https://cdn-icons-png.flaticon.com/512/91/91160.png")).
            ...
            arraylisError.add(Odontograma("https://cdn-icons-png.flaticon.com/512/91/91160.png","https://cdn-icons-png.flaticon.com/512/91/91160.png"))

            val adaptercliente = AdapterDientInfError(arraylisError, this, this)
            rcydienteinf?.layoutManager = LinearLayoutManager(this, LinearLayoutManager.HORIZONTAL, false)
            rcydienteinf?.adapter = adaptercliente

        }else{
            Toast.makeText(this,"Por favor selecciona un numero de Diente",Toast.LENGTH_SHORT).show()
        }


    }


    //FUNCTION onCLICKED
    fun Back(view: View){
            getOdontogramaSave(View(applicationContext),true)

    }

    fun help(view: View){

        if (imghelp){
            imghelp=false
            viewimghelp.visibility=View.VISIBLE
            carddientedent.visibility=View.GONE
            //lindienteinf.visibility=View.GONE
            carddientfuera.visibility=View.GONE
           // lindientesup.visibility=View.GONE
        }else if (!imghelp) {
            imghelp=true
            viewimghelp.visibility=View.GONE
            carddientedent.visibility=View.VISIBLE
          //  lindienteinf.visibility=View.VISIBLE
            carddientfuera.visibility=View.VISIBLE
        //    lindientesup.visibility=View.VISIBLE
        }


    }

    override fun onDientInfItemClick(url1: String) {
        btnhelp.visibility=View.VISIBLE
        carddientedent.visibility=View.VISIBLE
        lindienteinf.visibility=View.GONE
        var dientn= Ndient!!.toInt()-1
        val jsonObject= JSONArrayODONTOGRAMA!!.getJSONObject(dientn!!.toInt())
        val JsonOdont=JSONObject(jsonObject?.getString("d$Ndient").toString())

        Diente= ("{\n" +
                " \"d$Ndient\": ${"{\n" +
                        " \"url1\": \"${url1}\",\n" +
                        " \"url2\": \"${JsonOdont.getString("url2")}\"\n" +
                        " }"}\n" +
                " }")
        Glide.with(this)
            .load(url1)
            .into(dienteinf)
    }

    override fun onDientSupItemClick(url2: String) {
        btnhelp.visibility=View.VISIBLE

        carddientfuera.visibility=View.VISIBLE
        lindientesup.visibility=View.GONE
        var dientn= Ndient!!.toInt()-1
        val jsonObject= JSONArrayODONTOGRAMA!!.getJSONObject(dientn!!.toInt())
        val JsonOdont=JSONObject(jsonObject?.getString("d$Ndient").toString())


        Diente= ("{\n" +
                " \"d$Ndient\": ${"{\n" +
                        " \"url1\": \"${JsonOdont.getString("url1")}\",\n" +
                        " \"url2\": \"${url2}\"\n" +
                        " }"}\n" +
                " }")
        Glide.with(this)
            .load(url2)
            .into(dientesup)
    }

    override fun onItemClick(parent: AdapterView<*>?, view: View?, position: Int, id: Long) {
        Ndient = parent?.getItemAtPosition(position).toString().toInt()
        var dientn= Ndient!!.toInt()-1
        val jsonObject= JSONArrayODONTOGRAMA!!.getJSONObject(dientn!!.toInt())

        val JsonOdont=JSONObject(jsonObject?.getString("d$Ndient").toString())
            Glide.with(this)
                .load(JsonOdont.getString("url1").toString())
                .into(dienteinf)

            Glide.with(this)
                .load(JsonOdont.getString("url2").toString())
                .into(dientesup)

    }

}


```
Rest
```kotlin
    @GET("/appointment/odontograma.php?")
    suspend fun getListOdontograma(@Query("dni") dni:String,@Query("codigoprofesional") matricula:String):Response<List<OdontogramaRespons>>

    @GET("/appointment/odontograma.php?")
    suspend fun getOdontograma(@Query("dni") dni:String,@Query("codigoprofesional") matricula:String,@Query("fecha") fecha:String):Response<OdontogramaRespons>

    @FormUrlEncoded
    @POST("/appointment/odontograma.php")
    fun postOdontograma(
        @Field("dientes")  dientes:String,
        @Field("odontogramaid")  ID:Int,
        @Field("codigoprofesional")  idprofesional:String,
        @Field("dni")  dni:String,
        @Field("fecha")  fecha:String,
        @Field("insertar")  insertar:String,
        @Field("modificar")  modificar:String
    ): Call<OdontogramaRespons>


```
### Puesta en marcha












## Despues
Vista
```kotlin
fun Odontogram(
appointmentID:String,
viewModelappointment: AppointmentViewModel = hiltViewModel(),
viewModelfile: UserViewModel = hiltViewModel()
) {
    var appointment by remember {
        mutableStateOf(Appointment("","","","","","","","","",""))
    }
    var file by remember {
        mutableStateOf(Files("","","","","","","",""))
    }
    var odontogram by remember {
        mutableStateOf(app.ibiocd.appointment.model.Odontogram("","","",""))
    }
    val appointmentlist by viewModelappointment.appointment.observeAsState(arrayListOf())
    appointment= appointmentlist.find { appointmentID.contentEquals(it._id) }?: Appointment("","","","","","","","","","")

    val filelist by viewModelfile.files.observeAsState(arrayListOf())
    file=filelist.find { appointment.files.contentEquals(it._id) }?: Files("","","","","","","","")

    val odontogramlist by viewModelfile.odontogram.observeAsState(arrayListOf())
    val toothlist by viewModelfile.tooth.observeAsState(arrayListOf())


    odontogram=odontogramlist.find { file.odontogram.contentEquals(it._id) }?:Odontogram("","","","")


    //BUSCO EL TURNO Y BUSCO EL FILE Y BUSCO EL ODONTOGRAMA
    //BUSCO SI EXISTE UN ODONTOGRAMA PARA EL PACIENTE Y MEDICO, SI EXISTE ENTONCES PASO MOSTRAR LOS DATOS
    //EN CASO DE NO EXISTIR LO CREO
    // EL BOTON DE CREAR, ES DISTINTO QUE EL DE EDITAR POR TANT O SE BLOQUEA DEPENDIENDO SI EXISTE O NO

    var list= arrayListOf<Tooth>()


    var numbertooth by rememberSaveable{ mutableStateOf("") }
    var Bot by rememberSaveable { mutableStateOf(true) }
    var Top by rememberSaveable { mutableStateOf(false) }

    var SelectToothBot by rememberSaveable{ mutableStateOf("") }
    var SelectToothTop by rememberSaveable{ mutableStateOf("") }
    val listError= ArrayList<String>() //LISTA DE ERRORES DEL DIENTE DE ARRIBA

    for (i in 0 until 12){
        listError.add("https://cdn-icons-png.flaticon.com/512/91/91160.png")
    }

    Column(modifier = Modifier
        .fillMaxSize()
        .padding(30.dp)){
            dropDownTooth(list, onSelected = { select->


                numbertooth=select
                SelectToothBot= toothlist.find { it.number.contentEquals(numbertooth) }?.imgBot ?: ""
                SelectToothTop= toothlist.find { it.number.contentEquals(numbertooth) }?.imgTop ?: ""

            })


        LazyRow(modifier = Modifier
            .fillMaxWidth()
            .wrapContentWidth(Alignment.CenterHorizontally))
        {
            if (Top){
                item(){

                    Card(
                        shape = RoundedCornerShape(60.dp),
                        elevation = 20.dp,
                        modifier = Modifier
                            .height(280.dp)
                            .width(120.dp)
                            .padding(20.dp)
                            .clickable {
                                Top = !Top

                            }
                    ) {
                        Image(
                            modifier = Modifier
                                .fillMaxSize()
                                .clip(CircleShape),
                            painter = rememberImagePainter(
                                data = SelectToothTop,
                                builder = {
                                    placeholder(R.drawable.placeholder)
                                    error(R.drawable.placeholder)
                                }
                            ),
                            contentDescription = null,
                            contentScale = ContentScale.FillHeight
                        )


                    }
                }
            }else{

                val itemCount = listError.size

                //ITEMs DE LISTA DE ERROR
                items(count = itemCount){ index ->
                    Card(
                        shape = RoundedCornerShape(60.dp),
                        elevation = 20.dp,
                        modifier = Modifier
                            .height(280.dp)
                            .width(120.dp)
                            .padding(20.dp)
                            .clickable {


                                Top = !Top
                                SelectToothTop = listError[index]
                                toothlist.filter { it.number == numbertooth }.forEach { it.imgTop = SelectToothTop}

                            }

                    ) {


                    }
                }
            }
        }

        BlockSpace()
        LazyRow(modifier = Modifier
            .fillMaxWidth()
            .wrapContentWidth(Alignment.CenterHorizontally) )
        {
            if (Bot){
                item(){
                    Card(
                        shape = RoundedCornerShape(60.dp),
                        elevation = 20.dp,
                        modifier = Modifier
                            .height(130.dp)
                            .width(130.dp)
                            .padding(20.dp)
                            .clickable {
                                Bot = !Bot
                            }
                    ) {
                        Image(
                            modifier = Modifier
                                .fillMaxSize()
                                .clip(CircleShape),
                            painter = rememberImagePainter(
                                data = SelectToothBot,
                                builder = {
                                    placeholder(R.drawable.placeholder)
                                    error(R.drawable.placeholder)
                                }
                            ),
                            contentDescription = null,
                            contentScale = ContentScale.FillHeight
                        )


                    }
                }
            }else{
                //ITEMs DE LISTA DE ERROR
                val itemCount2 = listError.size

                items(count = itemCount2){ index ->
                    Card(
                        shape = RoundedCornerShape(60.dp),
                        elevation = 20.dp,
                        modifier = Modifier
                            .height(130.dp)
                            .width(130.dp)
                            .padding(20.dp)
                            .clickable {
                                Bot = !Bot

                                SelectToothBot = listError[index]
                                toothlist.filter { it.number == numbertooth }.forEach { it.imgBot = SelectToothBot}

                            }
                    ) {


                    }
                }
            }
        }



        Button(onClick = {
            if (file.odontogram==""){
                val listt= ArrayList<Tooths>()
                for (i in 0 until 32){
                    listt.add(
                        Tooths(i.toString(),
                        "https://appointmentibiocd.azurewebsites.net/Dientes/superior/sup${i}.png",
                            "https://appointmentibiocd.azurewebsites.net/Dientes/inferior/inf${i}.png"))
                }

                viewModelfile.addOdontogram(ApiTooth(listt),appointment.patient,appointment.medical)

            }else{

                viewModelfile.updateontogram(OdontogramResponse(odontogram._id,toothlist,odontogram.patient,odontogram.medical))

            }


        },modifier = Modifier
            .fillMaxWidth()
            .height(60.dp)
            .padding(horizontal = 30.dp),
            colors = ButtonDefaults.buttonColors(backgroundColor = Color.White),
            shape = RoundedCornerShape(30),
            elevation = ButtonDefaults.elevation(
                defaultElevation = 10.dp,
                pressedElevation = 8.dp,
                disabledElevation = 0.dp
            )
        ) {

            if (file.odontogram!=""){

                // if(state.value?.isLoading==true || isLoading) CircularProgressIndicator( modifier = Modifier.size(20.dp), strokeWidth = 2.dp)
                //else Text("Update")
                Text("Update")
            }else{

                // if(state.value?.isLoading==true || isLoading) CircularProgressIndicator( modifier = Modifier.size(20.dp), strokeWidth = 2.dp)
                //else Text("Create")
                Text("Create")
            }
        }


    }
}

@Composable
fun dropDownTooth(list: List<Tooth>, onSelected:(String) -> Unit) {
    var expanded by remember { mutableStateOf(false) }
    var selectedItem by remember { mutableStateOf("")}
    var textFiledSize by remember { mutableStateOf(Size.Zero)}

    val icon= if(expanded){
        painterResource(id = R.drawable.ic_keyboard_arrow_up)
    }else{
        painterResource(id = R.drawable.ic_keyboard_arrow_down)

    }
    Column() {
        OutlinedTextField(
            value = selectedItem,
            onValueChange = { selectedItem = it },
            label = { Text("Numero de diente") },
            modifier = Modifier
                .fillMaxWidth()
                .onGloballyPositioned { layoutCoordinates ->
                    textFiledSize = layoutCoordinates.size.toSize()
                },
            trailingIcon = {
                Icon( icon, contentDescription ="ToothList" , modifier = Modifier
                    .size(25.dp)
                    .clickable { expanded = !expanded }, tint = Color.Gray)
            }
        )

        DropdownMenu(
            expanded=expanded,
            onDismissRequest = {expanded=false},
            modifier = Modifier.width(with(LocalDensity.current){textFiledSize.width.toDp()})

        ){
            list.forEach { label ->
                DropdownMenuItem(onClick = {
                    selectedItem=label.number
                    onSelected(label.number)
                    expanded=false }) {
                    Text(text = label.number)
                }
            }
        }
    }
}

```
Funciones
ViewModel
```kotlin
     fun addOdontogram(data:ApiTooth, patient:String, medical:String) {

         if (_isLoading.value == false)
            viewModelScope.launch(Dispatchers.IO) {
                _isLoading.postValue(true)
                odontogramRepository.postOdontogram(data,patient,medical)
                _isLoading.postValue(false)
                Log.d("uploadOdontogram","Successful")
            }
    }

     fun loadOdontogram() {

         if (_isLoading.value == false)
            viewModelScope.launch(Dispatchers.IO) {
                _isLoading.postValue(true)
                odontogramRepository.getOdontogram()
                _isLoading.postValue(false)
                Log.d("loadOdontogram","Successful")
            }
    }
     fun deleteOdontogram() {

         if (_isLoading.value == false)
            viewModelScope.launch(Dispatchers.IO) {
                _isLoading.postValue(true)
                odontogramRepository.deleteAllOdontogram()
                _isLoading.postValue(false)
                Log.d("loadOdontogram","Successful")
            }
    }

     fun updateontogram(odontogram: OdontogramResponse) {

         if (_isLoading.value == false)
            viewModelScope.launch(Dispatchers.IO) {
                _isLoading.postValue(true)
                odontogramRepository.putOdontogram(odontogram)
                _isLoading.postValue(false)
                Log.d("uploadOdontogram","Successful")
            }
    }





```
Repository
```kotlin
interface OdontogramRepository {


    //PATIENT
    suspend fun putOdontogram(odontogram:OdontogramResponse): String
    suspend fun postOdontogram(data:ApiTooth,patient:String,medical:String): String
    suspend fun deleteOdontogram(toDelete: Odontogram)
    suspend fun deleteAllOdontogram()
    suspend fun getOdontogram()
    fun getAllOdontogram(): LiveData<List<Odontogram>>
    fun getAllTooth(): LiveData<List<Tooth>>


}

class OdontogramRepositoryImp @Inject constructor(
    private val dataSource: RestDataSource,
    private val odontogramDao: OdontogramDao

) : OdontogramRepository {

    override fun getAllOdontogram() = odontogramDao.getAllOdontogram()
    override fun getAllTooth() = odontogramDao.getAllTooth()
    override suspend fun putOdontogram(odontogram:OdontogramResponse): String {
        delay(3000)
        //DESCARGO LOS DATOS
        var  ID= ""

        val call = dataSource.putOdontogram(odontogram._id,odontogram.data.toString(),odontogram.patient,odontogram.medical)
        call.enqueue(object : Callback<UpdateResponse> {
            override fun onFailure(call: Call<UpdateResponse>, t: Throwable) {

                Log.d("puPatient -> ERROR", t.message.toString())

            }
            override fun onResponse(call: Call<UpdateResponse>, response: retrofit2.Response<UpdateResponse>) {

                Log.d("puPatient -> SUCCESSFULLY", response.body()?.acknowledged.toString())

            }
        })

        return ID
    }
    override suspend fun postOdontogram(data:ApiTooth,patient:String,medical:String): String {
        delay(3000)

        var arr= listOf<String>("ddca","2dsa","asd")
        //DESCARGO LOS DATOS


        Log.d("ARRAYDATA",data.data.toString())
        val call = dataSource.postOdontogram(RestDataSource.Toothsss(data.data,"",""))
        call.enqueue(object : Callback<RestDataSource.Toothsss> {
            override fun onFailure(call: Call<RestDataSource.Toothsss>, t: Throwable) {
                Log.d("ERROR",t.message.toString())

            }
            override fun onResponse(call: Call<RestDataSource.Toothsss>, response: retrofit2.Response<RestDataSource.Toothsss>) {
                //Log.d("Successful",response.body()?._id.toString())

            }
        })

        return ""
    }
    override suspend fun getOdontogram(){

        delay(3000)
        val listOdontogram= ArrayList<Odontogram>()
        val listTooth= ArrayList<Tooth>()
        for (i in 0 until dataSource.getOdontogram().result.size) {
            val _id =dataSource.getOdontogram().result[i]._id
            val data =dataSource.getOdontogram().result[i].data

           for (j in 0 until dataSource.getOdontogram().result[i].data.size) {
                val _id2 =dataSource.getOdontogram().result[i].data[j]._id
                val databot =dataSource.getOdontogram().result[i].data[j].imgBot
                val datatop =dataSource.getOdontogram().result[i].data[j].imgTop
                val datanum =dataSource.getOdontogram().result[i].data[j].number
                val tooth = Tooth(_id2,datanum,datatop, databot )//ESTRUCTURA DE DATOS PARA ROOM
                odontogramDao.insert(tooth)
               listTooth.add(tooth)

           }

            val patient =dataSource.getOdontogram().result[i].patient
            val medical =dataSource.getOdontogram().result[i].medical
            val odontogram = Odontogram(_id,"", patient, medical )//ESTRUCTURA DE DATOS PARA ROOM
            odontogramDao.insert(odontogram)
            listOdontogram.add(odontogram)
        }
        Log.d("getOdontogram","${listOdontogram.size} & ${listTooth.size}")


    }
    override suspend fun deleteOdontogram(toDelete: Odontogram) = odontogramDao.delete(toDelete)
    override suspend fun deleteAllOdontogram() {
        odontogramDao.deleteAllOdontogram()
        odontogramDao.deleteAllTooth()
        Log.d("deleteAllOdontogram","Successful")
    }

}

```

Rest
```kotlin

    @GET("odontogram")
    suspend fun  getOdontogram(): ApiOdontogram
    @FormUrlEncoded
    @POST("odontogram")
    suspend fun  postOdontogram(
        @Field("data") data:String,
        @Field("patient") patient:String,
        @Field("medical") medical:String
    ): Call<Odontogram>
    @FormUrlEncoded
    @PUT("odontogram/{id}")
    suspend fun  putOdontogram(
        @Path("id") id:String,
        @Field("data") data:String,
        @Field("patient") patient:String,
        @Field("medical") martes:String
    ): Call<UpdateResponse>
    @DELETE("odontogram/{id}")
    suspend fun  deleteOdontogram(@Path("id") id:String): Response<DeleteResponse>

```
### Puesta en marcha


