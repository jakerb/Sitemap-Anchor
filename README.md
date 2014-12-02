#Sitemap Anchor#
Automatic XML site map for Anchor CMS.

---
##Credit##
@terrillo originally [contributed the site map](http://terrillo.me/posts/how-to-create-a-seo-sitemap-for-anchor-cms) functionality, this post fixes the [XML validation bug](http://jakebown.com/index.php/posts/how-to-create-a-seo-sitemap-for-anchor-cms-fix).

##Adding the code##
Firstly, open up ` Anchor > routes > site.php` and paste the following before the **View Pages** section
 
<pre>/** * Sitemap */ Route::get('sitemap.xml', function() { $sitemap = ''; $sitemap .= ' ';
// Main page
$sitemap .= '\<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9"><url>';
$sitemap .= '<loc>' . Uri::full(Registry::get('posts_page')->slug . '/') . '</loc>';
$sitemap .= '<priority>0.9</priority>';
$sitemap .= '<changefreq>weekly</changefreq>';
$sitemap .= '</url>'; 
$query = Post::where('status', '=', 'published')->sort(Base::table('posts.created'), 'desc');
foreach($query->get() as $article) {
        $sitemap .= '<url>';
        $sitemap .= '<loc>' . Uri::full(Registry::get('posts_page')->slug . '/' . $article->slug) . '</loc>';
    $sitemap .= '<lastmod>' . date("Y-m-d", strtotime($article->created)) .'</lastmod>';
    $sitemap .= '<changefreq>monthly</changefreq>';
    $sitemap .= '<priority>0.8</priority>';
    $sitemap .= '</url>';
}
$sitemap .= '</urlset>';
return Response::create($sitemap, 200, array('content-type' => 'application/xml'));
});</pre>

That's it!