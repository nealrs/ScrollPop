#ScrollPop
ScrollPop is a small set of JS/PHP/CSS/HTML snippets which insert ads into the comment roll of your self-hosted WordPress blog. However, these aren't your run of the mill display ads. Ads are triggered only when you mouse or scroll through the comment roll. This reduces page weight, combats ad-blindness, improves accuracy of impressions, and helps you monetize user generated content.

As my first experiment with jQuery/WP hacking, I developed ScrollPop in July of 2010 while I was interning at Entertainment Weekly. ScrollPop also incorporates [Brandon Aaron](http://brandonaaron.net/)'s [jQuery.mousewheel plugin](https://github.com/brandonaaron/jquery-mousewheel).

ScrollPop is freely available under the MIT license.

##Demo 
There is a live demo of both mouse & scroll triggers available [here](http://stoutandporter.com). Additionally, the [AudioShocker Podcast](http://www.audioshocker.com) uses ScrollPop, (always on, no trigger), to power both frontpage merchandise ads and comment-roll promotions for several independent webcomics.

##Installation
To enable ScrollPop ads on your WP blog:
 
1. Copy and paste the php snippet into [comments.php](#commentsphp) file where noted.
2. Copy the css/js includes into the head block of your [header.php](#headerphp) file. 
3. Create and upload [sp.css](#spcss) & [sp.js](#spjss) to your theme's root directory.
4. Edit [sp.js](#spcss) with your desired ad creative & URLs.
5. [optional] Edit [comments.php](#commentsphp) to adjust the ad frequency, (default is 1 every 5 comments).

###comments.php
Place after .postcomment div, and before $comment_number++ statement at end of each comment.

```php
<?php
  $ad_frequency = 5;
	if ($comment_number % $ad_frequency == 0){
		$num_iterations = $comment_number / $ad_frequency;
		echo "<div class= 'adclear'></div><div class='adbox' id='adbox$num_iterations'><p>advertisement</p></div>";
	}
?>
```

###header.php

```HTML
<link rel="stylesheet" href="<?php echo get_stylesheet_directory_uri() ?>/sp.css" type="text/css" media="screen" />
<script src="//ajax.googleapis.com/ajax/libs/jquery/1.10.2/jquery.min.js"></script>
<script src="http://brandonaaron.github.io/jquery-mousewheel/jquery.mousewheel.js" type="text/javascript"></script>
<script src="<?php echo get_stylesheet_directory_uri() ?>/sp.js" type="text/javascript"></script>'
```

###sp.css

```css
.adbox {
  text-align: center;
  line-height:10px;
	font-size:10px;
	min-height:30px;
	overflow:hidden;
	padding: 0px;
	margin:10px 0 0 0;
	width:100%;
}
.adbox img {border:0;}
.adclear{clear: left;}</code></pre>
```

###sp.js

```js
mousead = 'http://nealshyam.com/scroll/iab.gif';
scrollurl = 'http://www.iab.org';
scrollad = 'http://nealshyam.com/scroll/iab2.gif';
mouseurl = 'http://www.google.com';
basead = 'http://nealshyam.com/scroll/iab.gif';
baseurl = 'http://www.google.com';

$(document).ready(function(){
	$(".adbox").mouseleave(function(){
		$(this).html('<p>advertisement</p><a href="'+mouseurl+'" target="_blank"><img src="'+mousead+'"></a>');		
	});
	
	$(".adbox").mousewheel(function(objEvent, delta){			  
		$(this).html('<p>advertisement</p><a href="'+scrollurl+'" target="_blank"><img src="'+scrollad+'"></a>');		
	});
	
	// comment out previous two statements and enable bottom statement to make ads 'always on'
	//$(".adbox").html('<p>advertisement</p><a href="'+baseurl+'" target="_blank"><img src="'+basead+'"></a>');		
});
```
