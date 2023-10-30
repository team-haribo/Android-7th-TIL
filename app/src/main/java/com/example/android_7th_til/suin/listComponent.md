Adapter
----------
- 데이터 <- Adapter -> view
- 데이터와 view 중간으로 **데이터 관리, view 생성**을 한다.

ListView
----------
- 리스트 형태의 데이터를 보여주는 위젯
- 리스트 형태 데이터를 adapter를 통해 데이터와 view를 연결한다.

#### 사용법
예시로 xml파일 3개를 각각 xml 1, 2, 3로   
kotlin파일 3개를 각각 kotlin 1, 2, 3로 표기

1. xml 파일에 listview 작성    
   [xml 1]
```xml
    <ListView
        android:id="@+id/NamNamMan"
    />
```
2. 데이터 클래스 정의    
   [kotlin 2]
```kt
data class Nam (
    val food : String, 
    val human : String
)
```
3. 데이터를 넣을 리스트 생성   
   [kotlin 1]
```kt
var MukBangList = arrayListOf<Nam>(
    Nam ( "딸기","human0" ),
    Nam ( "귤", "human1" )
)
```
4. 목록을 가지고 있는 ArrayList에 데이터 클래스를 넣는다.  
   [kotlin 1]
```kt
var MukBangList = arrayListOf<Nam>()
```
5. listview에 들어갈 항목을 만든다. ( EX : ImageView, TextView)   
   [xml 2]
```xml
    <ImageView
        android:id="@+id/humanPhotoIMG"
    />

    <TextView
        android:id="@+id/foodTV"
    />
```
6. 어댑터 생성
- 커스텀 Adapter은 BaseAdapter을 상속받음
- 오류 표시가 뜨면 Alt + Enter로 Override      
  [kotlin 3]
```kt
// MukBangList : 어댑터가 표시할 데이터
class MainAdapter (val context: Context,val MukBangList: Arraylist<Nam> ) : BaseAdapter() { 

    // getView(Int, View ViewGroup) : xml의 view와 데이터를 연결하는 메소드
    override fun getView(position: Int, convertView: View?, parent: ViewGroup?): View {

        // LayoutInflater : item을 adapter에서 사용할 view로 inflate역할을 함
        val view: View = LayoutInflater.from(context).inflate(R.layout.[kotlin 2])

        // 위 view를 다른 파일의 view와 연결
        val humanPhoto = view.findViewById<ImageView>(R.id.humanPhotoIMG)
        val foodNAME = view.findViewById<TextView>(R.id.foodTV)

        // ArrayList<Nam>의 사람 이미지랑 음식 이름을 view에 넣음
        val nam = MukBangList[position]
        val resourceId = context.resources.getIdentifier(human.photo, "drawable", context.packageName)
        humanPhoto.setImageResource(resourceId)
        foodName.text = food.name

        return view
    }

    // item을 반환 ( EX : 1번째 Nam을 고르고 싶으면 getItem(0) )
    override fun getItem(position: Int): Any {
        return MukBangList[position]
    }

    // 현재 위치의 id를 반환 ( 근데 이번엔 id가 필요가 없어 0으로 대체 )
    override fun getItemId(position: Int): Long {
        return 0
    }

    // listview에 있는 item 수를 반환
    override fun getCount(): Int {
        return MukBangList.size
    }
 }
```
7. 어댑터 설정   
   [kotlin 1]
```kt
class MainActivity : AppCompatActivity() {

    var MukBangList = arrayListOf<Nam>()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.[xml 1])
``
        val NamAdapter = MainListAdapter(this, MukBangList)
        mainListView.adapter = NamAdapter
    }
}
```

ListView
----------
- 스크롤을 내리면 위에 뷰를 없애고 아래 뷰에 데이터를 할당하는 방식
- Listview보다 재사용성이 좋음
- 리스트가 많아지면 비효율적
- viewholder 사용
    - 데이터가 틀 안에 들어갈 수 있는 기능을 정의
#### 사용법
1. recyclerView 정의   
   [build.gradle(app)]
```
implementation 'androidx.recyclerview:recyclerview:1.1.0'
```

[xml 1]
```xml
<androidx.recyclerview.widget.RecyclerView
    android:id="@+id/recyclerView"
/>
```
2. 아이템 레이아웃 생성   
   [xml 2]
```xml
<ImageView
    android:id="@+id/humanPhoto"
/>
<TextView
    android:id="@+id/foodTV"
/>
```
3. 데이터클래스 정의   
   [kotlin 1]
```kt
data class MukBangData(
    val human_img : String,
    val food_name : String
)
```
4. viewholder 생성 ( 바인딩 구현 ), adapter 생성   
   [kotlin 2] viewholder 생성
- 상속받는 viewholder 생성자에는 binding.root 전달 -> 홀더 내부에서 binding 사용 가능
```kt
inner class MyViewHolder(private val binding: ItemListBinding): RecyclerView.ViewHolder(binding.root) {
    //원래라면 class MyViewHolder(itemView:View):RecyclerView.Viewholder(itemView)
        fun bind(MukBangData:MukBangData){ 
            binding.humanPhotoImg.= MukBangData.human_img
            binding.foodTV.text= MukBangData.food_name
        }
    }
```
[kotlin 2] adapter 생성
```kt
class RecyclerViewAdapter: RecyclerView.Adapter<RecyclerViewAdapter.MyViewHolder>() {

    var datalist = mutableListOf<DogData>()//리사이클러뷰에서 사용할 데이터 미리 정의 -> 나중에 MainActivity등에서 datalist에 실제 데이터 추가

    --- viewholder 부분 ---
    inner class MyViewHolder(private val binding: ItemListBinding): RecyclerView.ViewHolder(binding.root) {
    //원래라면 class MyViewHolder(itemView:View):RecyclerView.Viewholder(itemView)
        fun bind(MukBangData:MukBangData){ 
            binding.humanPhotoImg.= MukBangData.human_img
            binding.foodTV.text= MukBangData.food_name
        }
    }


    //만들어진 뷰홀더 없을때 뷰홀더(레이아웃) 생성하는 함수
    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): MyViewHolder {
        val binding=[xml 2]Binding.inflate(LayoutInflater.from(parent.context),parent,false)
        return MyViewHolder(binding)
    }

    override fun getItemCount(): Int =datalist.size

    //recyclerview가 viewholder를 가져와 데이터 연결할때 호출
    //적절한 데이터를 가져와서 그 데이터를 사용하여 뷰홀더의 레이아웃 채움
    override fun onBindViewHolder(holder: MyViewHolder, position: Int) {
        holder.bind(datalist[position])
    }
  }
```
5. 최종 연결   
   [kotlin 1]
```kt
class MainActivity : AppCompatActivity() {

    private lateinit var binding:[xml 1]Binding 
    private lateinit var adapter:RecyclerViewAdapter // adapter 객체 먼저 선언

    val mDatas=mutableListOf<HumanData>()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding=[xml 1]Binding.inflate(layoutInflater)
        setContentView(binding.root)
      
        initializelist()
        initHumanRecyclerView()

    }

    fun initHumanRecyclerView(){
        val adapter=RecyclerViewAdapter() // 어댑터 생성
        adapter.datalist=mDatas // 데이터 넣기
        binding.recyclerView.adapter=adapter // 리사이클러뷰에 어댑터 연결
        binding.recyclerView.layoutManager=LinearLayoutManager(this) // 레이아웃 매니저 연결
    }

    // 데이터
    fun initializelist(){ 
        with(mDatas){
            add(DogData("","human1"))
            add(DogData("","human2"))
        }
    }
}
```
