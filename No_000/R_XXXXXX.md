# Playing with Paths in Flutter

### Links

[Playing with Paths in Flutter]()

### Notes

1. Use `CustomPaint` widget to provide a Canvas. And we need to use `Path` to draw on it.

Codes as below:

 ```dart
class LinePainter extends CustomPainter {
  final double progress;

  LinePainter({this.progress});

  Paint _paint = Paint()
    ..color = Colors.black
    ..strokeWidth = 4.0
    ..style = PaintingStyle.stroke
    ..strokeJoin = StrokeJoin.round;

  @override
  void paint(Canvas canvas, Size size) {
    var path = Path();
    path.moveTo(0, size.height / 2);
    path.lineTo(size.width * progress, size.height / 2);
    canvas.drawPath(path, _paint);
  }

  @override
  bool shouldRepaint(LinePainter oldDelegate) {
    return oldDelegate.progress != progress;
  }
}
 ```

2. Use `PathMetric` to measuring a path and extract sub-path:

   Code as below:

   ```dart
   
   class DashLinePainter extends CustomPainter {
     final double progress;
   
     DashLinePainter({this.progress});
   
     Paint _paint = Paint()
       ..color = Colors.black
       ..strokeWidth = 4.0
       ..style = PaintingStyle.stroke
       ..strokeJoin = StrokeJoin.round;
   
     @override
     void paint(Canvas canvas, Size size) {
       var path = Path()
         ..moveTo(0, size.height / 2)
         ..lineTo(size.width * progress, size.height / 2);
   
       Path dashPath = Path();
   
       double dashWidth = 10.0;
       double dashSpace = 5.0;
       double distance = 0.0;
   
       for (PathMetric pathMetric in path.computeMetrics()) {
         while (distance < pathMetric.length) {
           dashPath.addPath(
             pathMetric.extractPath(distance, distance + dashWidth),
             Offset.zero,
           );
           distance += dashWidth;
           distance += dashSpace;
         }
       }
       canvas.drawPath(dashPath, _paint);
     }
   
     @override
     bool shouldRepaint(DashLinePainter oldDelegate) {
       return oldDelegate.progress != progress;
     }
   }
   ```

3. Use `path.addOval(Rect.fromCircle(center: Offset(0,0), radius:80.0))` to draw a circle 

