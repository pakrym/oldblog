---
layout: post
title: "EditGeometry with point moving support for Esri ArcGis Silverlight API"
date: 2013-01-08 10:56
comments: true
categories: arcgis silverlight
---

`EditGeometry` class which is shiped with ArcGis Silverlight API is great to edit all kind of geometries except MapPoint which need only move. So I've inherited EditGeometry class and added point moving functionality. 

**Usage**

1. Call `Init(Map)` method on the map.
2. When you need to edit geometry call `StartEditEx`
3. To stop or cancel edit call `StopEditEx` or `CancelEditEx` respectively.

{% codeblock lang:csharp %}
 public class EditGeometryExtended : EditGeometry
    {

        public bool MovingPoint { get; set; }
        public MapPoint InitialLocation { get; set; }
        public Graphic SelectedGraphics { get; set; }


        public void Init(Map map)
        {
            map.MouseMove += Move;
            foreach (var layer in map.Layers)
            {
                var graphicsLayer = layer as GraphicsLayer;
                if (graphicsLayer != null)
                {
                    graphicsLayer.MouseLeftButtonDown += LeftDown;
                    graphicsLayer.MouseLeftButtonUp += LeftUp;
                }
            }
        }

        public void StartEditEx(Graphic graphic)
        {
            if (graphic == null) throw new ArgumentNullException("graphic");

            var point = graphic.Geometry as MapPoint;
            if (point != null)
            {
                SelectedGraphics = graphic;
            }
            else
            {
                StartEdit(graphic);
            }
        }

        public void CancelEditEx()
        {
            if (SelectedGraphics == null) return;
            
            var point = SelectedGraphics.Geometry as MapPoint;
            if (point != null)
            {
                SelectedGraphics.Geometry = InitialLocation;
                InitialLocation = null;
                SelectedGraphics = null;
            }
            else
            {
                CancelEdit();
            }
        }

        public void StopEditEx()
        {
            if (SelectedGraphics == null) return;
            

            var point = SelectedGraphics.Geometry as MapPoint;
            if (point != null)
            {
                InitialLocation = null;
                SelectedGraphics = null;
            }
            else
            {
                CancelEdit();
            }

        }

        private void Move(object sender, MouseEventArgs e)
        {
            if (SelectedGraphics == null) return;
            
            var point = SelectedGraphics.Geometry as MapPoint;
            if (MovingPoint && point != null)
            {
                SelectedGraphics.Geometry = Map.ScreenToMap(e.GetPosition(Map));
            }
        }

        private void LeftUp(object sender, GraphicMouseButtonEventArgs e)
        {
            if (SelectedGraphics == null) return;
            
          
            var point = SelectedGraphics.Geometry as MapPoint;
            if (point != null)
            {
                MovingPoint = false;
            }
        }

        private void LeftDown(object sender, GraphicMouseButtonEventArgs e)
        {
            if (SelectedGraphics == null) return;
            
          
            var point = SelectedGraphics.Geometry as MapPoint;
            if (point != null)
            {
                InitialLocation = point;
                MovingPoint = true;
                e.Handled = true;
            }
        }
    }
{% endcodeblock %}