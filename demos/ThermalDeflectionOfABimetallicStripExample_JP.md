# Thermal Deflection of Bimetallic Beam


���̗�ł́A�M�e�������������@���Љ�܂��B�@�B�\���̔M�c���܂��͎��k�́A������̉��x�ω��ɂ�蔭�����܂��B�M���͓͂񎟓I�Ȍ��ۂł��F�\����̐���ɂ��R���|�[�l���g�̎��R�ȔM�c���܂��͎��k���W������ƁA�\���ɉ��͂�������܂��B�悭���镨�������Ƃ��ăo�C���^���r�[���̂���݂����グ�܂��B�T�^�I�ȃo�C���^���r�[���́A�݂��Ɍ������ꂽ2�̍ޗ��ō\������Ă��܂��B�����̍ޗ��̔M�c���W���iCTE�Fcoefficients of thermal expansion�j�͑傫���قȂ�܂��B




![image_0.png](ThermalDeflectionOfABimetallicStripExample_JP_images/image_0.png)




���̗�ł́A�\���L���v�f���f�����g�p���ăo�C���^���r�[���̂���݂��v�Z���A�r�[�����_�ߎ��Ɋ�Â�����͉��Ɣ�r���Ă��܂��B 




�ÓI�\�����f�����쐬���܂��B


```matlab
structuralmodel = createpde('structural','static-solid');
```


���̐��@�̃r�[���`����쐬���܂��B


```matlab
L = 0.1; % m
W = 5E-3; % m
H = 1E-3; % m
gm = multicuboid(L,W,[H,H],'Zoffset',[0,H]);
```


�\�����f���Ɍ`��̏���ǉ����܂��B


```matlab
structuralmodel.Geometry = gm;
```


�`����v���b�g���܂��B


```matlab
figure
pdegplot(structuralmodel)
```

![figure_0.png](ThermalDeflectionOfABimetallicStripExample_JP_images/figure_0.png)



�ޗ��������w�肷��Z���̃��x������肵�܂��B




�܂���ԉ��̃Z���̃��x����\�����܂��B���x���𖾊m�ɕ\������ɂ́A�r�[���̍��[���Y�[�����A���̂悤�Ɍ`�����]�����܂��B


```matlab
figure
pdegplot(structuralmodel,'CellLabels','on')
axis([-L/2 -L/3 -W/2 W/2 0 2*H])
view([0 0])
zticks([])
```

![figure_1.png](ThermalDeflectionOfABimetallicStripExample_JP_images/figure_1.png)



���ɁA��̃Z�����x����\�����܂��B�Z�����x���𖾊m�ɕ\������ɂ́A�r�[���̉E�[���Y�[�����A���̂悤�Ɍ`�����]���܂��B


```matlab
figure
pdegplot(structuralmodel,'CellLabels','on')
axis([L/3 L/2 -W/2 W/2 0 2*H])
view([0 0])
zticks([])
```

![figure_2.png](ThermalDeflectionOfABimetallicStripExample_JP_images/figure_2.png)



�����O���A�|�A�\����A����ѐ��`�M�c���W�����w�肵�āA���`�e���ޗ��̋��������f�������܂��B�P�ʌn�̈�ѐ����ێ�����ɂ́A���ׂĂ̕����v���p�e�B�� SI ���j�b�g�Ŏw�肵�܂��B 




���̍ޗ������������Z���Ɋ��蓖�Ă܂��B


```matlab
Ec = 137E9; % N/m^2
nuc = 0.28;
CTEc = 20.00E-6; % m/m-C
structuralProperties(structuralmodel,'Cell',1, ...
                                     'YoungsModulus',Ec, ...
                                     'PoissonsRatio',nuc, ...
                                     'CTE',CTEc);
```


�C���o�[�i�s�ύ|�j�̍ޗ��������㕔�Z���Ɋ��蓖�Ă܂��B


```matlab
Ei = 130E9; % N/m^2
nui = 0.354;
CTEi = 1.2E-6; % m/m-C
structuralProperties(structuralmodel,'Cell',2, ...
                                     'YoungsModulus',Ei, ...
                                     'PoissonsRatio',nui, ...
                                     'CTE',CTEi);
```


���̗�ł́A�r�[���̍��[���Œ肳��Ă���Ɖ��肵�܂��B���̋��E�������ۂ����߂́A�܂��r�[���̍��[�ɖʃ��x����\�����܂��B


```matlab
figure
pdegplot(structuralmodel,'faceLabels','on','FaceAlpha',0.25)
axis([-L/2 -L/3 -W/2 W/2 0 2*H])
view([60 10])
xticks([])
yticks([])
zticks([])
```

![figure_3.png](ThermalDeflectionOfABimetallicStripExample_JP_images/figure_3.png)



�� 5 �� 10 �ɌŒ�[�Ƃ���������^���܂��B


```matlab
structuralBC(structuralmodel,'Face',[5,10],'Constraint','fixed');
```


���x�ω���M���ׂƂ��ēK�p���܂��B�ێ�25�x�̊���x�Ɛێ�125�x�̓��쉷�x��z�肵�܂��B���������āA���̃��f���̉��x���͐ێ�100�x�ł��B


```matlab
structuralBodyLoad(structuralmodel,'Temperature',125);
structuralmodel.ReferenceTemperature = 25;
```


���b�V���𐶐����������߂܂��B


```matlab
generateMesh(structuralmodel,'Hmax',H/2);
R = solve(structuralmodel);
```


�ψʂ̑傫�����J���[�}�b�v�f�[�^�Ƃ��ăo�C���^���r�[���̕ό`��̌`����v���b�g���܂��B


```matlab
figure
pdeplot3D(structuralmodel,'ColorMapData',R.Displacement.Magnitude, ...
                          'Deformation',R.Displacement, ...
                          'DeformationScaleFactor',2)
title('Deflection of Invar-Copper Beam')
```

![figure_4.png](ThermalDeflectionOfABimetallicStripExample_JP_images/figure_4.png)



�r�[�����_�Ɋ�Â��Ă���݂���͓I�Ɍv�Z���܂��B����݂� <img src="https://latex.codecogs.com/gif.latex?\inline&space;\delta&space;=\frac{6\Delta&space;\;T\left(\alpha_c&space;-\alpha_i&space;\right)\;L^{2\;}&space;}{K_1&space;}"/>, ������ <img src="https://latex.codecogs.com/gif.latex?\inline&space;K_1&space;=14+\frac{E_c&space;}{E_i&space;}+\frac{E_i&space;}{E_c&space;}"/>, <img src="https://latex.codecogs.com/gif.latex?\inline&space;\Delta&space;\;T"/> �͉��x��, <img src="https://latex.codecogs.com/gif.latex?\inline&space;\alpha_c"/> �� <img src="https://latex.codecogs.com/gif.latex?\inline&space;\alpha_i"/> �͓��ƃC���o�[�̔M�c���W��, <img src="https://latex.codecogs.com/gif.latex?\inline&space;E_c"/> �� <img src="https://latex.codecogs.com/gif.latex?\inline&space;E_i"/> �͓��ƃC���o�[�̃����O��, <img src="https://latex.codecogs.com/gif.latex?\inline&space;L"/> �̓r�[���̒����ł��B


```matlab
K1 = 14 + (Ec/Ei)+ (Ei/Ec);
deflectionAnalytical = 3*(CTEc - CTEi)*100*2*H*L^2/(H^2*K1);
```


��͉��Ɛ��l�v�Z�œ���ꂽ�l���r���܂��B


```matlab
PDEToobox_Deflection = max(R.Displacement.uz);
percentError = 100*(PDEToobox_Deflection - ...
                    deflectionAnalytical)/PDEToobox_Deflection;

bimetallicResults = table(PDEToobox_Deflection, ...
                          deflectionAnalytical,percentError);
bimetallicResults.Properties.VariableNames = {'PDEToolbox', ...
                                              'Analytical', ...
                                              'PercentageError'};
disp(bimetallicResults)
```
```
    PDEToolbox    Analytical    PercentageError
    __________    __________    _______________

    0.0071061     0.0070488         0.80608    
```


*Copyright 2018 The MathWorks, Inc.*


