---
layout: post
title: "Railsのform_tag"
description: ""
category: Rails
tags: []
---
Railsでアプリを作る時、Modelがなく、単純にFormを作りたい場合、`form_tag`を使うべきという感じ

WEB開発は全くないので、書き方すらよくわかってない。

何かを作りたいとしても、あんまり進められない

    <%= form_tag(:action => "disp") do -%>
      Birthday<br/>
            <%= select_date Date.today,
                :prefix => "birthday",
                :use_month_numbers => true,
                :start_year => 1980,
                :end_year => 2012,
                :discard_day => true %><br/>
            <%= submit_tag "display", :class => "btn btn-primary" %>
    <% end -%>
