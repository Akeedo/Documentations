# ২১.৭ - ড্যাশবোর্ড উইজেট তৈরী করা

এই অধ্যয় আমরা ড্যাশবোর্ড উইজেট তৈরী করব ।  এই জন্য আমরা একটা  ফোল্ডার বানাব আর নাম দিবো  DashboardWidget। তারপর আমরা একটা ফাইল বানব আর নাম দিবো dashboardwidget.php ।  



![](E:\Office\scripts, lists and docs\my repository\documentation\Plugin Development\plugin_development\21-7\1.PNG)









dashboardwidget.php এর ফাইল এ গিয়ে আমরা  php opening tag দিয়ে আমরা আগে plugin ইনফরমেশন দিই 

````php+HTML
<?php
/**
 * Plugin Name: DemoDashboard Widget
 * Description: DemoDashboard Widget
 * Plugin URI: https://learnwith.hasinhayder.com
 * Author: Akeed Anum
 * Author URI: https://learnwith.hasinhayder.com
 * Version: 1.0
 * License: GPL2 or later
 * License URI: https://www.gnu.org/licenses/gpl-2.0.html
 * Text Domain: dashboardwidget
 * Domain Path: Domain Path
 * Network: false
 */
````

প্রথমে আমরা plugin_loaded action hook টি দিবো তো আমরা ব্যবহার `add_action('plugins_loaded','ddw_load_textdomain');` করবো ।

তারপর আমরা এই action hook উপরে আমরা ddw_load_textdomain function টা অ্যাড করবো । 

```php+HTML
function ddw_load_textdomain(){
load_plugin_textdomain('dashboardwidget',false,plugin_dir_path(__FILE__)."/languages");
}
```

তারপর আমরা একটা language নামের folder বানাবো । 



![](E:\Office\scripts, lists and docs\my repository\documentation\Plugin Development\plugin_development\21-7\2.PNG)



তারপর আমরা add_action hook বানাবো আর $tag হিসাবে আমরা  wp_dashboard_setup দিবো আর $function_to_add হবে ddw_dashboard_widget । তারপর আমরা ddw_dashboard_widget function টা বানাবো  এই add_action hook এর উপরে । এখুন ddw_dashboard_widget এই function_body টে আমরা wp_add_dashboard_widget অ্যাড করবো , আর এই function's  () notation ভিতোরে আমরা $widget_id দিছি  demodashboardwidget আর $widget_name ট্রান্সলেট করে  নামটা দিছি তাই আমরা ট্রান্সলেশন এ $text কে DashboardWidget দিবো আর $domain কে dashboardwidget দিবো , তারপর আমরা $callback এর জন্য আমরা ddw_dashboardwidget_output আমরা পারামিটার হিসেবে দিই । 

````php+HTML
function ddw_dashboard_widget(){
	wp_add_dashboard_widget('demodashboardwidget',
		__('DashboardWidget','dashboardwidget'),
		'ddw_dashboardwidget_output'
	);
}
````

তারপর আমরা  ddw_dashboardwidget_output function বানাবো আর এই function_body তে আমরা `echo "Hello World";` লিকব ।

তারপর browser এ গিয়ে আমরা dashboard click করে দেকব আমদের widget টা আছে ।

**Note According to the Mentor -** 

https://www.youtube.com/watch?v=UiO3TmnjB7Q&feature=youtu.be&ab_channel=AkeedAnjumoffice

এখুন আমরা ওই ddw_dashboardwidget_output function_body আগের কোড বাদ দিয়ে একটা array নিবনি ।  array ভিতরে তে আমরা 4 টা key আর value নিবনি । 

- 'url' => 'https://wptavern.com/feed/json'
- 'items' => 5
- 'show_summary' => 1
- 'show_author' => 0
- 'show_date' => 1

এই array টা আর একটা  arrray ভিতরে মধ্যে দিবনি আর এটা এসাইন করবনি একটা  variable  সঙ্গে আর variable এর নাম হবে $feeds । তারপর আমরা wp_dashboard_primary_output অ্যাড করবো আর ()notation এ parameter হিসাবে 'dashboardwidget' দিবো $widget_id হিসেবে আর $feeds দিবো $feeds হিসাবে । 

সামগ্রিকভাবে কোড হবে

````php+HTML
function ddw_dashboardwidget_output(){
	$feeds = array(
		array(
			'url' => 'https://wptavern.com/feed/podcast',
			'items' => 5,
			'show_summary' => 1,
			'show_author' => 0,
			'show_date' => 1
		)
	);
	wp_dashboard_primary_output( 'dashboardwidget', $feeds );
}
````



![](E:\Office\scripts, lists and docs\my repository\documentation\Plugin Development\plugin_development\21-7\4.PNG)





21-8

According to the mentor in this video

https://youtu.be/i4ZC4ayb3Zg

তাই আমরা  ddw_dashboard_widget function_body তে wp_add_dashboard_widget এর ()notation ভিতরে আমরা 'ddw_dashboardwidget_configure' অ্যাড করব $control_callback জন্য । এখুন আমরা  ddw_dashboardwidget_configure ফাংশনটা বানাব তৈরি করেছি । আর function_body তে আমরা `echo "Configure";`টাইপ করব । 

<img src="E:\Office\scripts, lists and docs\my repository\documentation\plugin_development\21-8\1.PNG" />

 

<span style="font-size:2rem; background:red; color:white">**Note-**</span>

According to the mentor:

<iframe width="560" height="315" src="https://www.youtube.com/embed/Z2yxEZ9g78E" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

তাই আমরা এখুন একটা if loop দিবো আর boolean expression এ ' current_user_can function টা দিবো যেটার ()notation ভিতরে 'edit_dashboard' দিবো $capability হিসাবে । 

![](E:\Office\scripts, lists and docs\my repository\documentation\plugin_development\21-8\3.png) 

সুতরাং পূর্ণাঙ্গ কোড হবে ddw_dashboard_widget function এ 

````php+HTML
function ddw_dashboard_widget(){
	if(current_user_can( 'edit_dashboard' )){
		wp_add_dashboard_widget( 'demodashboardwidget',
		__('Dashboard Widget', 'dashboardwidget' ),
		'ddw_dashboardwidget_output',
		'ddw_dashboardwidget_configure'
	);
	}else{
		wp_add_dashboard_widget( 'demodashboardwidget',
		__('Dashboard Widget', 'dashboardwidget' ),
		'ddw_dashboardwidget_output'
		);
	}
 }
````

তাহলে admin ছারে অন্য users রা দেখতে পারবে না  এই configure টা 

![](E:\Office\scripts, lists and docs\my repository\documentation\plugin_development\21-8\1.PNG)

আখুন আমরা ddw_dashboardwidget_configure function_body ভিতরে আমরা  get_option function টা দিবো আর ()notation ভিতরে $option এ dashboardwidget_nop দিবো আর $default হিসাবে 5 দিবো । তারপর php closing tag দিবো । দিয়ে আমরা paragraph opening  দিবনি , তারপর  content এ label আর input opening আর closing tag দিবনি । label tag দিবো আর এর content এ  আমরা লিখব Number of Posts: । পরবর্তী ধাপ এ আমরা input tag  নিবো , এটার সঙ্গে 4 টা attribute দিয়ে value দিবো ।

- type="text"
-  name="ddw_nop" 
- id="ddw_nop" 
- class="widfat"
- value="<?php echo $number_of_posts;?>"

তারপর  paragraph closing tag দিবনি । তারপর php closing tag দিবো 

সুতরাং ddw_dashboardwidget_configure function_body  পূর্ণাঙ্গ কোড হবে

````php+HTML
function ddw_dashboardwidget_configure(){
	$number_of_posts = get_option('dashboardwidget_nop',5);
	?>
	<p>
		<label>Number of Posts:</label><br/>
		<input type="text" name="ddw_nop" id="ddw_nop" class="widefat" value="<?php echo $number_of_posts;?>">
	</p>
	<?php
}
````

তারপর আমরা browser এ গিয়ে দেখি 

<img src="E:\Office\scripts, lists and docs\my repository\documentation\plugin_development\21-8\4.PNG" style="zoom:100%;" />



কিন্তু আরমা input এ 10 দিয়ে submit button ক্লিক করে ও কিছু হয় না কারন এটার জন্য আমদের value প্রসেস করা লাগবে তাই আমরা 









