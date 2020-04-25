# add arabic languege to reportlab
> this repo show how to add arabic language text to a pdf file using reportlab

### here is the  code :
```python
from reportlab.lib.styles import getSampleStyleSheet
from reportlab.lib.pagesizes import letter
from reportlab.platypus import Paragraph, SimpleDocTemplate, Spacer,PageBreak
from reportlab.lib.styles import ParagraphStyle
from reportlab.pdfbase import pdfmetrics
import reportlab
from reportlab.pdfbase.ttfonts import TTFont
###################################################

import arabic_reshaper

from bidi.algorithm import get_display


#init the style sheet
styles = getSampleStyleSheet()

    #### add custome font ####
pdfmetrics.registerFont(TTFont('Arabic', '29ltbukraregular.ttf'))

    ####### STYLE #######
#heading style
styleH_1 = styles['Heading1']

## paragraph style
styleN = styles['Normal']
square_text_style = ParagraphStyle(
    'border',
    parent = styleN ,
    borderColor= '#333333',
    borderWidth =  1,
    borderPadding  =  2,
)

arabic_text_style = ParagraphStyle(
    'border',
    parent = styleN ,
    borderColor= '#333333',
    borderWidth =  1,
    borderPadding  =  2,
    fontName="Arabic"
)
#reshape the text
arabic_text =""" عندما يريد العالم أن ‪يتكلّم ‬ ، فهو يتحدّث بلغة
 يونيكود. تسجّل الآن لحضور المؤتمر الدولي العاشر ليونيكود (Unicode Conference)، الذي سيعقد في 10-12 آذار 1997 بمدينة مَايِنْتْس، ألمانيا. و سيجمع المؤتمر بين خبراء
  من كافة قطاعات الصناعة على الشبكة العالمية انترنيت ويونيكود، حيث ستتم، على الصعيدين الدولي والمحلي على حد سواء مناقشة سبل استخدام يونكود في النظم القائمة وفيما يخص التطبيقات الحاسوبية، الخطوط، تصميم النصوص والحوسبة متعددة اللغات."""
rehaped_text = arabic_reshaper.reshape(arabic_text)
bidi_text = get_display(rehaped_text)

##########################"  add element to the story ####################
story = []

##arabic text showing squares 
story.append(Paragraph("Arabic text showing black squares ",styleH_1))
story.append(Spacer(1,8))
story.append(Paragraph(bidi_text,square_text_style))
story.append(Spacer(1,14))


#perfect arabic text
story.append(Paragraph("Perfect arabic text ",styleH_1))
story.append(Spacer(1,8))
story.append(Paragraph(bidi_text,arabic_text_style))

# save the pdf file
doc = SimpleDocTemplate('mydoc.pdf',pagesize = letter)
doc.build(story)
```
