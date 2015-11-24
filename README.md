# CSS Optimizer for PHP

[![Build Status](https://travis-ci.org/lastzero/css-optimizer.png?branch=master)](https://travis-ci.org/lastzero/css-optimizer)
[![Latest Stable Version](https://poser.pugx.org/lastzero/css-optimizer/v/stable.svg)](https://packagist.org/packages/lastzero/css-optimizer)
[![Total Downloads](https://poser.pugx.org/lastzero/css-optimizer/downloads.svg)](https://packagist.org/packages/lastzero/css-optimizer)
[![License](https://poser.pugx.org/lastzero/css-optimizer/license.svg)](https://packagist.org/packages/lastzero/css-optimizer)

Minifies stylesheets by removing duplicates and combining selectors - mostly safe and very efficient. This package was developed to optimize the CSS of an existing commercial Web site, as the CSS contained many duplicates and similar selectors created by the [Sass](http://sass-lang.com/) CSS precompiler (I don't want to blame Sass, but that's the story). 

Input:

    body { foo:bar; too: me; } 
    body { baz:foo; }
    body { foo: new }
    div.test { border: 1px solid black; }
    div.other { border: 1px solid black}

Output:

    body{baz:foo;foo:new;too:me}div.other,div.test{border:1px solid black}

Usage:

    $optimizer = new \CssOptimizer\Css\Optimizer;
    
    $inputCss = 'body { foo:bar; too: me;} body { baz:foo; } body { foo: new }
    div.test { border: 1px solid black; }
    div.other { border: 1px solid black} ';
    
    $minifiedCss = $optimizer->minifyCss($inputCss);
    $optimizedCss = $optimizer->optimizeCss($minifiedCss);
