Android  VS IOS
=============

##语法

| Java | Swift
|:------:|:----------:
| import packagename.Xyz; | import framework| class Xyz extends SomeClass | class Xyz : SomeClass| interface Abc | protocol Abc| class Xyz extends SomeClass implements Abc | class Xyz: SomeClass,Abc 
| int mProperty; | varmProperty: Int| Xyz() // constructor | init()| Xyz obj = new Xyz(); | var obj : Xyz = Xyz()| void doWork(String arg) | funcdoWork (arg: String)->Void| obj.doWork(arg); | obj.doWork(arg)| Access Modifier: private vs. public | private vs. public

##通用语法
| 通用 | Java | Swift
|:------:|:----------:|:------:
| Self | this.aMember | self.aMember
| Variables | String aName | var aName: String
| Boolean | boolean | Bool
| Integer | Integer or int | Int, UInt
| Null value | null | nil
| Array | ArrayList or JSONArray if string serialization is required | Array or NSArray| Hash table | HashMap or JSONObject if string serialization is required | Dictionary or NSDictionary

##结构
| Android | IOS
|:------:|:----------:
| Layout files | Content view 
| Fragment class | UIViewController
| Java event listener| Delegate
| Activity class that coordinates the child Fragments in it | Container view controller
| Android Layout file | storyboard
| RelativeLayout | Auto Layout
| rootView.findViewById(...), setOnXxxListener(...) | IBOutlet, IBAction
| FragmentTransaction | UINavigationController

##生命周期

| Android(Fragment) | IOS
|:------:|:----------:
| onCreate() & onCreateView() | viewDidLoad()
| onStart() | viewWillAppear()
| onResume() | viewDidAppear()
| onPause() | viewWillDisappear()
| onStop() | viewDidDisappear()


##UI

| Android | IOS
|:------:|:----------:
| View, ViewGroup | UIView
| TextView | UILabel
| EditText(Single Line) | UITextField
| EditText(Multiple Line) | UITextView
| Button | UIButton
| RadioGroup | UISegmentedControl
| SeekBar | UISlider
| ProgressBar (default style) | UIActivityIndicatorView
| ProgressBar (horizontal style) | UIProgressView
| Switch, Checkbox, ToggleButton | UISwitch
| ImageView | UIImageView
| Options Menu, ActionBar Items | UIBarButtonItem
| Context Menu, PopupMenu | UIActionSheet
| Spinner | UIPickerView
| WebView | UIWebView
| ScrollView | UIScrollView
| ListView | UITableView 
| ListView Item | UITableViewCell
| Adapter | UITableViewDataSource 
| ListFragment.onListItemClick(...) | UITableViewDelegate
| GridView | UICollectionView
| VideoView | MPMoviePlayerViewController

##UI controller 

| Android | IOS
|:------:|:----------:
| Toasts | -
| ListFragment | UITableViewController 
| ViewPager | UIPageViewController
| AlertDialog | UIAlertController
| DialogFragment | UIPopoverController
| Actionbar Navigation Tabs | UITabBarController

##数据

| Android | IOS
|:------:|:----------:
| App Resources | Assets Catalog, Externalize Strings
| SharedPreferences | NSUserDefaults
| File | NSFileManager

##网络

| Android | IOS
|:------:|:----------:
| HttpURLConnection | NSURLConnection


