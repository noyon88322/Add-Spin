


## Switch case problem

PieView

   ```bash


package com.example.watchandearn.spin;

import android.content.Context;
import android.graphics.Canvas;
import android.graphics.Paint;
import android.graphics.RectF;
import android.graphics.drawable.Drawable;
import android.util.AttributeSet;
import android.util.TypedValue;
import android.view.View;

import java.nio.file.attribute.AttributeView;
import java.util.Calendar;
import java.util.List;

public class PieView extends View {

    private RectF  range = new RectF();

    public int radius;
    private Paint mArcPaint,mBackgroundPaint,mTextPaint;
    private float mStartAngle = 0;
    private int center,padding,targetIndex,roundOfNumber =4;

    private boolean isRunning = false;
    private int defaultBackgroundColor = -1;
    private Drawable drawableCenterImage;
    private int textColor = 0xffffffff;

    private List<SpinItem>spinItemList;
    private PieRotateListener pieRotateListener;

    private interface PieRotateListener{
        void rotateDone(int index);

    }

    public PieView(Context context){
        super(context);
    }
    private PieView(Context context, AttributeSet attr){
        super(context,attr);

    }


    public void setPieRotateListener(PieRotateListener listener){

        this.pieRotateListener = listener;

    }

    private void init(){
        mArcPaint = new Paint();
        mArcPaint.setAntiAlias(true);
        mArcPaint.setDither(true);


        mTextPaint = new Paint();
        mTextPaint.setColor(textColor);
        mTextPaint.setTextSize(TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_SP
        ,14
        ,getResources().getDisplayMetrics()));

        range = new RectF(padding,padding,padding+radius,padding+radius);
    }
    public void setData(List<SpinItem> spinItemsList){

        this.spinItemList = spinItemsList;
        invalidate();

    }
    public void setPieCenterImage(Drawable drawable){

        drawableCenterImage = drawable;
        invalidate();

    }

    public void setPieTextColor(int color){
        textColor = color;
        invalidate();
    }

    protected void onDraw(Canvas canvas){

        if (spinItemList == null){
            return;
        }
        drawBackgroundColor(canvas,defaultBackgroundColor);
        init();

        float tampAngle = mStartAngle;
        float sweepAngle = 360 / spinItemList.size();

        for (int i = 0; i<spinItemList.size(); i++){

            mArcPaint.setColor(spinItemList.get(i).color);

            canvas.drawArc(range,tampAngle,sweepAngle,true,mArcPaint);
            drawText(canvas,tampAngle,sweepAngle, spinItemList.get(i).text);
            tampAngle += sweepAngle;


        }

        drawCenterImage(canvas,drawableCenterImage);


    }

    private void drawBackgroundColor(Canvas canvas, int color){


        if(color == -1){
            return;
        }
        mBackgroundPaint = new Paint();
        mBackgroundPaint.setColor(color);
        canvas.drawCircle(center,center,center,mBackgroundPaint);

    }

    protected void onMeasure(int widthMeasureSp, int hightMeasureSp){
        super.onMeasure(widthMeasureSp,hightMeasureSp);

        int width = Math.min(getMeasuredWidth(),getMeasuredHeight());

        padding = getPaddingLeft() == 0 ? 10: getPaddingLeft();

        radius = width-padding*2;
        center = width/2;
        setMeasuredDimension(width,width);

    }





}


   ```

   ## SpinItem



   ```bash

   package com.example.watchandearn.spin;

public class SpinItem {

    String text;
    int color,icon;

}


   ```

## WheelUtils



   ```bash

   package com.example.watchandearn.spin;

import android.graphics.Bitmap;
import android.graphics.Canvas;
import android.graphics.drawable.BitmapDrawable;
import android.graphics.drawable.Drawable;

public class WheelUtils {

    public static Bitmap bitmapToDrawable(Drawable drawable){


        Bitmap bitmap = null;

        if (drawable instanceof BitmapDrawable){
            BitmapDrawable bitmapDrawable = (BitmapDrawable) drawable;

            if (bitmapDrawable.getBitmap() != null){
                return bitmapDrawable.getBitmap();
            }

        }
        if (drawable.getIntrinsicWidth() <= 0 || drawable.getIntrinsicHeight() <= 0){
            bitmap = Bitmap.createBitmap(1,1,Bitmap.Config.ARGB_8888);

        }
        else {
            bitmap = Bitmap.createBitmap(drawable.getIntrinsicWidth(),drawable.getIntrinsicHeight(),Bitmap.Config.ARGB_8888);

        }

        Canvas canvas =new Canvas();
        drawable.setBounds(0,0,canvas.getWidth(),canvas.getHeight());
        drawable.draw(canvas);
        return bitmap;

    }

}


   ```

   


