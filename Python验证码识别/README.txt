�����÷ָ�ͼƬ + ����ʶ��ķ�ʽ��ʵ���˼��׵���֤���ƽ⡣

Ȼ�����ڵ���֤��Խ��Խ���ӣ����ǵķ������Դ����˸��Ӹ��ӵ���֤�롣

��ʱ�����Ҫ������ԭ�еĻ����������µ��б�ʽ������ϵͳ����Ӧ�ԡ�


# �������:

from PIL import Image

im = Image.open("captcha.gif")
#(��ͼƬת��Ϊ8λ����ģʽ)
im.convert("P")

#��ӡ��ɫֱ��ͼ
print im.histogram()
�����

[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0 , 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 2, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 2, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 2, 1, 0, 0, 0, 2, 0, 0, 0, 0, 1, 0, 1, 1, 0, 0 , 1, 0, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 2, 0, 0, 0, 1, 2, 0, 1, 0, 0, 1, 0, 2, 0, 0, 1, 0, 0, 2, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 1, 0, 3, 1, 3, 3, 0, 0, 0, 0, 0, 0, 1, 0, 3, 2, 132, 1, 1, 0, 0, 0, 1, 2, 0, 0, 0, 0, 0, 0, 0, 15, 0 , 1, 0, 1, 0, 0, 8, 1, 0, 0, 0, 0, 1, 6, 0, 2, 0, 0, 0, 0, 18, 1, 1, 1, 1, 1, 2, 365, 115, 0, 1, 0, 0, 0, 135, 186, 0, 0, 1, 0, 0, 0, 116, 3, 0, 0, 0, 0, 0, 21, 1, 1, 0, 0, 0, 2, 10, 2, 0, 0, 0, 0, 2, 10, 0, 0, 0, 0, 1, 0, 625]
��ɫֱ��ͼ��ÿһλ���ֶ���������ͼƬ�к��ж�Ӧλ����ɫ�����ص�������

ÿ�����ص�ɱ���256����ɫ����ᷢ�ְ׵�����ࣨ��ɫ���255��λ�ã�Ҳ�������һλ�����Կ�������625����ɫ���أ��������������200���ң����ǿ���ͨ�����򣬵õ����õ���ɫ��

his = im.histogram()
values = {}

for i in range(256):
    values[i] = his[i]

for j,k in sorted(values.items(),key=lambda x:x[1],reverse = True)[:10]:
    print j,k
�����

255 625
212 365
220 186
219 135
169 132
227 116
213 115
234 21
205 18
184 15
���ǵõ���ͼƬ������10����ɫ������ 220 �� 227 ����������Ҫ�ĺ�ɫ�ͻ�ɫ������ͨ����һѶϢ����һ�ֺڰ׶�ֵͼƬ��

#-*- coding:utf8 -*-
from PIL import Image

im = Image.open("captcha.gif")
im.convert("P")
im2 = Image.new("P",im.size,255)


for x in range(im.size[1]):
    for y in range(im.size[0]):
        pix = im.getpixel((y,x))
        if pix == 220 or pix == 227: # these are the numbers to get
            im2.putpixel((y,x),0)

im2.show()
�õ��Ľ����

1-4.1

4.2 ��ȡ�����ַ�ͼƬ
�������Ĺ�����Ҫ�õ������ַ������ؼ��ϣ��������ӱȽϼ򵥣����Ƕ�����������и

inletter = False
foundletter=False
start = 0
end = 0

letters = []

for y in range(im2.size[0]): 
    for x in range(im2.size[1]):
        pix = im2.getpixel((y,x))
        if pix != 255:
            inletter = True
    if foundletter == False and inletter == True:
        foundletter = True
        start = y

    if foundletter == True and inletter == False:
        foundletter = False
        end = y
        letters.append((start,end))

    inletter=False
print letters
�����

[(6, 14), (15, 25), (27, 35), (37, 46), (48, 56), (57, 67)]
�õ�ÿ���ַ���ʼ�ͽ���������š�

import hashlib
import time

count = 0
for letter in letters:
    m = hashlib.md5()
    im3 = im2.crop(( letter[0] , 0, letter[1],im2.size[1] ))
    m.update("%s%s"%(time.time(),count))
    im3.save("./%s.gif"%(m.hexdigest()))
    count += 1
(������Ĵ���)

��ͼƬ�����и�õ�ÿ���ַ����ڵ��ǲ���ͼƬ��

4.3 AI �������ռ�ͼ��ʶ��
����������ʹ�������ռ��������������ַ�ʶ�������кܶ��ŵ㣺

����Ҫ������ѵ������
����ѵ������
�������ʱ���룯�Ƴ���������ݲ鿴Ч��
���������ͱ�д�ɴ���
�ṩ�ּ����������Բ鿴��ӽ��Ķ��ƥ��
�����޷�ʶ��Ķ���ֻҪ���뵽���������У����Ͼ���ʶ���ˡ�
��Ȼ��Ҳ��ȱ�㣬���������ٶȱ����������ܶ࣬�������ҵ��Լ��ķ����������ȵȡ�

���������ռ����������ԭ����Բο���ƪ���£�http://ondoc.logand.com/d/2697/pdf

Don't panic�������ռ�����������������ȥ�ܸߴ�����ʵԭ��ܼ򵥡����������������˵��

���� 3 ƪ�ĵ�������Ҫ��ô��������֮������ƶ��أ�2 ƪ�ĵ���ʹ�õ���ͬ�ĵ���Խ�࣬������ƪ���¾�Խ���ƣ������ⵥ��̫����ô�죬����������ѡ�񼸸��ؼ����ʣ�ѡ��ĵ����ֱ�����������ÿһ�������ͺñȿռ��е�һ��ά�ȣ�x��y��z �ȣ���һ����������һ��ʸ����ÿһ���ĵ����Ƕ��ܵõ���ôһ��ʸ����ֻҪ����ʸ��֮��ļнǾ��ܵõ����µ����ƶ��ˡ�

�� Python ��ʵ�������ռ䣺

import math

class VectorCompare:
    #����ʸ����С
    def magnitude(self,concordance):
        total = 0
        for word,count in concordance.iteritems():
            total += count ** 2
        return math.sqrt(total)

    #����ʸ��֮��� cos ֵ
    def relation(self,concordance1, concordance2):
        relevance = 0
        topvalue = 0
        for word, count in concordance1.iteritems():
            if concordance2.has_key(word):
                topvalue += count * concordance2[word]
        return topvalue / (self.magnitude(concordance1) * self.magnitude(concordance2))
����Ƚ����� python �ֵ����Ͳ�������ǵ����ƶȣ��� 0��1 �����ֱ�ʾ��

4.4 ��֮ǰ�����ݷ���һ��
����ȡ������֤����ȡ�����ַ�ͼƬ��Ϊѵ�����ϵĹ�������ֻҪ���кúö����ĵ�ͬѧ��һ��֪����Щ����Ҫ��ô�������������ȥ�ˡ�����ֱ��ʹ���ṩ��ѵ����������������Ĳ�����

iconsetĿ¼�·ŵ������ǵ�ѵ������

���׷�ӵ����ݣ�

#��ͼƬת��Ϊʸ��
def buildvector(im):
    d1 = {}
    count = 0
    for i in im.getdata():
        d1[count] = i
        count += 1
    return d1

v = VectorCompare()

iconset = ['0','1','2','3','4','5','6','7','8','9','0','a','b','c','d','e','f','g','h','i','j','k','l','m','n','o','p','q','r','s','t','u','v','w','x','y','z']

#����ѵ����
imageset = []
for letter in iconset:
    for img in os.listdir('./iconset/%s/'%(letter)):
        temp = []
        if img != "Thumbs.db" and img != ".DS_Store":
            temp.append(buildvector(Image.open("./iconset/%s/%s"%(letter,img))))
        imageset.append({letter:temp})


count = 0
#����֤��ͼƬ�����и�
for letter in letters:
    m = hashlib.md5()
    im3 = im2.crop(( letter[0] , 0, letter[1],im2.size[1] ))

    guess = []

    #���и�õ�����֤��СƬ����ÿ��ѵ��Ƭ�ν��бȽ�
    for image in imageset:
        for x,y in image.iteritems():
            if len(y) != 0:
                guess.append( ( v.relation(y[0],buildvector(im3)),x) )

    guess.sort(reverse=True)
    print "",guess[0]
    count += 1