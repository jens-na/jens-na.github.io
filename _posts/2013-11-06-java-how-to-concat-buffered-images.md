---
layout: post
title: Concatenate images (one below the other) in Java
---

Recently I was faced with the problem to concatenate images with Java. To be
more specific I needed to concatenate `BufferedImage` objects (one below the
other). Afterwards I had to export the new image as a PNG file.

First of all we have to create a few dummy images and store them in an array.
{% highlight java %}
        int imagesCount = 4;
        BufferedImage images[] = new BufferedImage[imagesCount];
        for(int j = 0; j < images.length; j++) {
            images[j] = new BufferedImage(100, 100, BufferedImage.TYPE_INT_RGB);
            Graphics2D g2d = images[j].createGraphics();
            g2d.drawRect(10, 10, 80, 80);
            g2d.dispose();
        } 
{% endhighlight %}

Afterwards we have to calculate the total height of all images in the array
and store the value in the variable `heightTotal`.

{% highlight java %}
        int heightTotal = 0;
        for(int j = 0; j < images.length; j++) {
            heightTotal += images[j].getHeight();
        }
{% endhighlight %}

We can finally create our new `BufferedImage` and append the dummy images. The
variable `heightCurr` helps us to save the current height.

{% highlight java %}
        int heightCurr = 0;
        BufferedImage concatImage = new BufferedImage(100, heightTotal, BufferedImage.TYPE_INT_RGB);
        Graphics2D g2d = concatImage.createGraphics();
        for(int j = 0; j < images.length; j++) {
            g2d.drawImage(images[j], 0, heightCurr, null);
            heightCurr += images[j].getHeight();
        }
        g2d.dispose();
{% endhighlight %}

To export your image(s) you can use `ImageIO`.

{% highlight java %}
        ImageIO.write(concatImage, "png", new File("/home/jens/concat.png")); // export concat image
        ImageIO.write(images[0], "png", new File("/home/jens/single.png")); // export single image
{% endhighlight %}

**Result**

Single image

![concat image](/images/java-concat-single.png)

Concatenated image

![concat image](/images/java-concat-concat.png)
