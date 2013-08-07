<!-- DO NOT EDIT THIS FILE; it is auto-generated from readme.txt -->
# Dependency Minification

Automatically concatenates and minifies any scripts and stylesheets enqueued using the standard dependency system.

**Contributors:** [x-team](http://profiles.wordpress.org/x-team), [westonruter](http://profiles.wordpress.org/westonruter)  
**Tags:** [dependencies](http://wordpress.org/plugins/tags/dependencies), [minify](http://wordpress.org/plugins/tags/minify), [concatenate](http://wordpress.org/plugins/tags/concatenate), [compress](http://wordpress.org/plugins/tags/compress), [js](http://wordpress.org/plugins/tags/js), [javascript](http://wordpress.org/plugins/tags/javascript), [scripts](http://wordpress.org/plugins/tags/scripts), [css](http://wordpress.org/plugins/tags/css), [styles](http://wordpress.org/plugins/tags/styles), [stylesheets](http://wordpress.org/plugins/tags/stylesheets), [gzip](http://wordpress.org/plugins/tags/gzip), [yslow](http://wordpress.org/plugins/tags/yslow), [pagespeed](http://wordpress.org/plugins/tags/pagespeed)  
**Requires at least:** 3.5  
**Tested up to:** 3.6  
**Stable tag:** trunk (master)  
**License:** [GPLv2 or later](http://www.gnu.org/licenses/gpl-2.0.html)  

## Description ##

This plugin takes all scripts and stylesheets that have been added via `wp_enqueue_script` and `wp_enqueue_style`
and *automatically* concatenates and minifies them into logical groups. For example, scripts in the footer get grouped
together and styles with the same media (e.g. `print`) get minified together. Minification is done via WP-Cron in order
to prevent race conditions and to ensure that the minification process does not slow down page responses.

This is a reincarnation and rewrite of the [Optimize Scripts](http://wordpress.org/plugins/optimize-scripts/) plugin,
which this plugin now supersedes.

Features:

 * Minified sources are stored in the WP Options table so no special filesystem access is required.
 * Endpoint for minified requests is at `/_minified`, which can be configured.
 * Admin page for taking inventory of minified scripts and stylesheets, with methods for expiring or purging the cached data.
 * Dependencies which must not be minified may be excluded via the `dependency_minification_excluded` filter.
 * Dependencies hosted on other domains are by default excluded, but this behavior can be changed by filtering the `default_exclude_remote_dependencies` option via the `dependency_minification_options` filter, or on a case-by-case basis via the filter previously mentioned.
 * By default excludes external scripts from being concatenated and minified, but they can be opted in via the `dependency_minification_excluded` filter.
 * The length of time that a minified source is cached defaults to 1 month, but can be configured via the `cache_control_max_age_cache` option.
 * If a minified source is not available yet, the page source will note that the dependency minification process is pending.
 * Any errors that occur during minification will be shown on the frontend in comments if the `show_error_messages` option is enabled; such errors are enabled by default if `WP_DEBUG`.
 * If the minification process errors out, the original unminified sources are served and the error is cached for 1 hour (by default, configured via `cache_control_max_age_error`) to prevent back-to-back crons from continually attempting to minify in perpetuity.
 * Cached minified sources are served with `Last-Modified` and `ETag` responses headers and requests will honor `If-None-Match` and `If-Modified-Since` to return `304 Not Modified` responses (configurable via the `allow_not_modified_responses` option).
 * Data attached to scripts (e.g. via `wp_localize_script`) is also concatenated together and attached to the newly-minified script.
 * WP-Cron is utilized to initiate the minification process in order to prevent race conditions, ensure page responses aren't slowed down.
 * Stale minified scripts and stylesheets remain until replaced by refreshed ones; this ensures that full-page caches which reference stale minified sources won't result in any 404s.
 * Can serve compressed responses with `gzip` or `deflate`.

Development of plugin is done on GitHub: [https://github.com/x-team/wp-dependency-minification](https://github.com/x-team/wp-dependency-minification)

Pull requests welcome.

If you are using Nginx with the default Varying Vagrant Vagrants config, you'll want to remove `css` and `js` from this rule:

    # Handle all static assets by serving the file directly. Add directives 
    # to send expires headers and turn off 404 error logging.
    location ~* \.(js|css|png|jpg|jpeg|gif|ico)$ {
        expires 24h;
        log_not_found off;
    }

## Changelog ##

### 0.9 beta ###
First Release

