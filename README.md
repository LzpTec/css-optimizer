# CSS Optimizer for PHP

[![Build Status](https://travis-ci.org/lastzero/css-optimizer.png?branch=master)](https://travis-ci.org/lastzero/css-optimizer)
[![Latest Stable Version](https://poser.pugx.org/lastzero/css-optimizer/v/stable.svg)](https://packagist.org/packages/lastzero/css-optimizer)
[![License](https://poser.pugx.org/lastzero/css-optimizer/license.svg)](https://packagist.org/packages/lastzero/css-optimizer)

Minifies stylesheets by removing duplicates and combining selectors - mostly safe and very efficient. This package was developed as a  "quick win" to optimize the CSS of an existing commercial Web site, as the CSS contained many duplicates and similar selectors created by the [Sass](http://sass-lang.com/) CSS precompiler.

*Note: The optimization is performed using simple (and fast) string functions - it's not a real CSS parser. It requires well formed CSS without inline comments (don't worry, you can use the minifyCss() method for that, see usage).*

*Warning: The order of selectors in your CSS does play a role and the "further down" one does in fact win when the specificity values are exactly the same. If your CSS relies on order to work, this optimization will obviously break it. See also https://en.wikipedia.org/wiki/Cascading_Style_Sheets#Specificity*

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
    
    print_r($optimizer->getCounts());
    

Assetic filter
--------------

This library comes with a ready-to-use Assetic filter (for Symfony2). Please use a pre-filter like CssMinFilter.

Usage:

    $filter = new \CssOptimizer\Assetic\Filter\CssOptimizeFilter;
    $asset = new StringAsset($inputCss);
    $asset->ensureFilter($filter);
    $outputCss = $asset->dump();
