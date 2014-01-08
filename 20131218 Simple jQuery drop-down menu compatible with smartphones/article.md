# Simple jQuery drop-down menu compatible with smartphones

One of the most essential things in a website is a menu. In this article you will read about a very easy way to create drop-down menus that also work nicely with smartphones and tablets.

The logic behind the script is simple. We have menu header elements and menu contents. Menu headers are visible. Menu contents become visible when the mouse moves over the headers or when the user clicks on them, like in a touch screen case.

What connects headers with contents is their HTML ID. For example there is a header element named `menu_home` and a content element named `menu_home_content`. The way the script scans the page for elements to connect is through the CSS class of the header elements. In this script, menu headers must have a CSS class named `menu_header`.

So practically speaking, in order to make the system work you have to add a header element with a HTML ID of your choice, a content element with an ID that adds "_content" to the header ID and a CSS class `menu_header`.


## HTML example

    <div class="menu">
        <span class="menu_header" id="menu_home">Home</span>
        <span class="menu_header" id="menu_blog">Blog</span>
        <span class="menu_header" id="menu_about">About</span>
        <span class="menu_header" id="menu_contact">Contact</span>
    </div>

    <div class="menu_content" id="menu_home_content">
        <p>This is the content panel for the Home menu item</p>
    </div>

    <div class="menu_content" id="menu_about_content">
        <p>This is the content panel for the About menu item</p>
    </div>

    <div class="menu_content" id="menu_contact_content">
        <p>This is the content panel for the Contact menu item</p>
    </div>

One small detail you may notice in the example above is that the content for the Blog menu item is missing. This may be a valid case of a menu that does not have a content. The script searches for content, if there isn't one it's not a problem.


**Compatibility with touch screen devices**  
The key to make a traditional drop-down menu compatible to smartphones and tablets is simply make it responsive to mouse clicks. The script in this article does that. Mouse clicks open or close menus just as mouse-over or mouse-out events do.


## jQuery script

    jQuery(document).ready(function () {

        (function ($) {
            this.active_item = null;

            this.hide_timeout = 100;
            this.hide_timer = 0;

            this.open_item = function (obj_header, obj_content) {
                this.active_item = obj_content[0];
                var pos = obj_header.offset();
                obj_content.css({left: pos.left, top: pos.top + obj_header.height()}).stop().fadeIn();
            }

            var _menu = this;

            $(".menu_header").each(function () {
                if (this.id) {
                    var obj_content = $("#" + this.id + "_content");
                    if (obj_content.length > 0) {
                        obj_content.appendTo($("body")).css({position: "absolute"});

                        var obj_header = $(this);

                        obj_header
                            .mouseover(function () {
                                clearTimeout(_menu.hide_timer);
                                _menu.hide_timer = 0;

                                if (_menu.active_item != null && _menu.active_item != obj_content[0]) {
                                    $(_menu.active_item).stop().hide();
                                    _menu.active_item = null;
                                }

                                if (_menu.active_item == null) {
                                    _menu.open_item(obj_header, obj_content);
                                }
                            })
                            .mouseout(function () {
                                _menu.hide_timer = setTimeout(
                                    function () {
                                        _menu.hide_timer = 0;
                                        $(_menu.active_item).stop().fadeOut();
                                        _menu.active_item = null;
                                    },
                                    _menu.hide_timeout
                                );
                            })
                            .click(function () {
                                if (_menu.active_item == null) {
                                    _menu.open_item(obj_header, obj_content);
                                }
                                else {
                                    $(_menu.active_item).stop().fadeOut();
                                    _menu.active_item = null;
                                }
                            });

                        obj_content
                            .mouseover(function () {
                                clearTimeout(_menu.hide_timer);
                                _menu.hide_timer = 0;
                            })
                            .mouseout(function () {
                                _menu.hide_timer = setTimeout(
                                    function () {
                                        _menu.hide_timer = 0;
                                        $(_menu.active_item).stop().fadeOut();
                                        _menu.active_item = null;
                                    },
                                    _menu.hide_timeout
                                );
                            });
                    }
                }
            });
        })(jQuery);

    });

This article was originally published in [http://www.creativepulse.gr/en/blog/2013/simple-jquery-drop-down-menu-compatible-with-smartphones](http://www.creativepulse.gr/en/blog/2013/simple-jquery-drop-down-menu-compatible-with-smartphones)

