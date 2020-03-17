# Heat Conduction in Multidomain Geometry with Nonuniform Heat Flux


���̗�ł́A3�̈قȂ�ޗ��w�łł������󋅂�3�����ߓn�M�`����͂����s������@�������܂��B 




���͕̂s�ψ�ȊO���M�����̉e�����󂯂܂��B




���̖��̕����I�����ƌ`��́ASingh, Suneet, P. K. Jain, and Rizwan-uddin�i�Q�l�������Q�Ɓj�Ő�������Ă���A���̖��̉�͉�������܂��B���̓��ʂ̉��x�͏�Ƀ[���ł��B�O�������iy �����l�j�ɂ́A���̎��Œ�`�����s�ψ�ȔM����������܂��B



<img src="https://latex.codecogs.com/gif.latex?q_{{{outer}}}&space;=\theta^2&space;(\pi&space;-\theta&space;)^2&space;\phi^2&space;(\pi&space;-\phi&space;)^2"/>


<img src="https://latex.codecogs.com/gif.latex?0\le&space;\theta&space;\le&space;\pi&space;,0\le&space;\phi&space;\le&space;\pi&space;."/>



<img src="https://latex.codecogs.com/gif.latex?\inline&space;\theta"/> �� <img src="https://latex.codecogs.com/gif.latex?\inline&space;\phi"/> �͋����̕��ʊp�Ƌp�ł��B�ŏ��́A���̂̂��ׂĂ̓_�̉��x�̓[���Ƃ��܂��B


# ���f����`


�ߓn�M��͗p�̔M���f�����쐬���܂��B


```matlab
thermalmodel = createpde('thermal','transient');
```


`multisphere` �֐����g�p���đ��w�����쐬���܂��B�ł����`�����ō�����M���f���Ɋ��蓖�Ă܂��B���̂ɂ́A����̓����R�A������3�̍ޗ��w������܂��B


```matlab
gm = multisphere([1,2,4,6],'Void',[true,false,false,false]);
thermalmodel.Geometry = gm;
```


�`����v���b�g���A�Z���Ɩʂ̃��x����\�����܂��B `FaceAlpha` �� 0.25 �ɐݒ肵�āA�����̃��x�����\�������悤�ɂ��܂��B


```matlab
figure('Position',[10,10,800,400]);
subplot(1,2,1)
pdegplot(thermalmodel,'FaceAlpha',0.25,'CellLabel','on')
title('Geometry with Cell Labels')
subplot(1,2,2)
pdegplot(thermalmodel,'FaceAlpha',0.25,'FaceLabel','on')
title('Geometry with Face Labels')
```

![figure_0.png](UnsteadyHeatConductionInAMultilayeredSphereExample_JP_images/figure_0.png)

# ���b�V���쐬


���b�V���𐶐����܂��B�����ł͏������x�����������邽�߂ɑe���i�`��𐳊m�ɕ\���̂ɏ\���ɍׂ����j���b�V���T�C�Y�ɂ��܂��B


```matlab
generateMesh(thermalmodel,'Hmax',1);
```


���̂̊e�w�̔M�`�����A���ʖ��x�A����є�M���w�肵�܂��B�����ł͍ޗ������͖������̒l�ł���A���ۂ̍ޗ������l�ł͂Ȃ��_�ɒ��ӂ��Ă��������B


```matlab
thermalProperties(thermalmodel,'Cell',1,'ThermalConductivity',1, ...
                                        'MassDensity',1, ...
                                        'SpecificHeat',1);
thermalProperties(thermalmodel,'Cell',2,'ThermalConductivity',2, ...
                                        'MassDensity',1, ...
                                        'SpecificHeat',0.5);
thermalProperties(thermalmodel,'Cell',3,'ThermalConductivity',4, ...
                                        'MassDensity',1, ...
                                        'SpecificHeat',4/9);
```
# ���E������`


���E�������w�肵�܂��B�ł������̖ʂ̉��x�͏�Ƀ[���ł��B


```matlab
thermalBC(thermalmodel,'Face',1,'Temperature',0);
```


���̊O�ʂɂ͊O���M����������܂��B�M�������`���邽�߂ɁA�M�I���E������^����֐����g�p���܂��B


```matlab
function Qflux = externalHeatFlux(region,~)
    [phi,theta,~] = cart2sph(region.x,region.y,region.z);
    theta = pi/2 - theta; % transform to 0 <= theta <= pi
    ids = phi > 0;
    Qflux = zeros(size(region.x));
    Qflux(ids) = theta(ids).^2.*(pi - theta(ids)).^2.*phi(ids).^2.*(pi - phi(ids)).^2;
end
```


�\�ʂ̔M�������v���b�g���܂��B


```matlab
[phi,theta,r] = meshgrid(linspace(0,2*pi),linspace(-pi/2,pi/2),6);
[x,y,z] = sph2cart(phi,theta,r);
region.x = x;
region.y = y;
region.z = z;
flux = externalHeatFlux(region,[]);
figure
surf(x,y,z,flux,'LineStyle','none')
axis equal
view(130,10)
colorbar
xlabel 'x'
ylabel 'y'
zlabel 'z'
title('External Flux')
```

![figure_1.png](UnsteadyHeatConductionInAMultilayeredSphereExample_JP_images/figure_1.png)



���̋��E���������f���ɒǉ����܂��B


```matlab
thermalBC(thermalmodel,'Face',4,'HeatFlux',@externalHeatFlux,'Vectorized','on');
```
# ��������


���������F���ׂĂ̓_�ŏ������x���[���ɂȂ�悤�ɒ�`���܂��B


```matlab
thermalIC(thermalmodel,0);
```
# ����


�M���z�̉ߓn�I�ω���ǂ����߁A���ԃX�e�b�v�x�N�g�����`���������߂܂��B


```matlab
tlist = [0,2,5:5:50];
R = solve(thermalmodel,tlist);
```


�������̃��x�������ׂẴv���b�g�œ����ɂ��邽�߂ɁA���߂�ꂽ���̒��ł̉��x�͈͂����߂܂��B�ŏ����x�͋��̓��ʂ̋��E�����ł���[���ł��B


```matlab
Tmin = 0;
```


�ŏI�^�C���X�e�b�v�̉�����ō����x�������܂��B


```matlab
Tmax = max(R.Temperature(:,end));
```
# ����


`tlist` �Œ�`���ꂽ�����ɂ����� `Tmin` ���� `Tmax` �͈̔͂œ��������v���b�g���܂��B


```matlab
h = figure;
for i = 1:numel(tlist)
    pdeplot3D(thermalmodel,'ColorMapData',R.Temperature(:,i))
    caxis([Tmin,Tmax])
    view(130,10)
    title(['Temperature at Time ' num2str(tlist(i))]);
    M(i) = getframe;
end
```

![figure_2.png](UnsteadyHeatConductionInAMultilayeredSphereExample_JP_images/figure_2.png)



�����\������ɂ͈ȉ��̃R�}���h�����s���Ă��������B�i2 ��Đ����܂��j


```matlab
movie(M,2)
```


�f�ʉ��x�̓������}��`���܂��B�܂��A<img src="https://latex.codecogs.com/gif.latex?\inline&space;x=0"/> �ɂ����� <img src="https://latex.codecogs.com/gif.latex?\inline&space;y-z"/> ���ʂ̒����`�O���b�h���`���܂��B


```matlab
[YG,ZG] = meshgrid(linspace(-6,6,100),linspace(-6,6,100));
XG = zeros(size(YG));
```


�O���b�h��ł̉��x����}���ċ��߂܂��B�f�ʉ��x�̓������}�̕ω����ώ@���邽�߂ɁA�������̎����ɂ����鉷�x���z����}���ċ��߂܂��B


```matlab
tIndex = [2,3,5,7,9,11];
varNames = {'Time_index','Time_step'};
index_step = table(tIndex.',tlist(tIndex).','VariableNames',varNames);
disp(index_step);
```
```
    Time_index    Time_step
    __________    _________

         2            2    
         3            5    
         5           15    
         7           25    
         9           35    
        11           45    
```
```matlab
TG = interpolateTemperature(R,XG,YG,ZG,tIndex);
```


�f�ʂɊe�w�̋��E��\���~��\�����邽�߂̏����F


```matlab
t = linspace(0,2*pi);
ylayer1 = cos(t); zlayer1 = sin(t);
ylayer2 = 2*cos(t); zlayer2 = 2*sin(t);
ylayer3 = 4*cos(t); zlayer3 = 4*sin(t);
ylayer4 = 6*cos(t); zlayer4 = 6*sin(t);
```


���ԃC���f�b�N�X `tIndex` �ɑΉ����鎞���ɂ����鉷�x�̓��������A `Tmin` ���� `Tmax` �͈̔͂Ńv���b�g���܂��B


```matlab
figure('Position',[10,10,1000,550]);
for i = 1:numel(tIndex)
    subplot(2,3,i)
    contour(YG,ZG,reshape(TG(:,i),size(YG)),'ShowText','on')
    colorbar
    title(['Temperature at Time ' num2str(tlist(tIndex(i)))]);
    hold on
    caxis([Tmin,Tmax])
    axis equal
    % Plot boundaries of spherical layers for reference.
    plot(ylayer1,zlayer1,'k','LineWidth',1.5)
    plot(ylayer2,zlayer2,'k','LineWidth',1.5)
    plot(ylayer3,zlayer3,'k','LineWidth',1.5)
    plot(ylayer4,zlayer4,'k','LineWidth',1.5)
end
```

![figure_3.png](UnsteadyHeatConductionInAMultilayeredSphereExample_JP_images/figure_3.png)

# Helpter function
```matlab
function Qflux = externalHeatFlux(region,~)
    [phi,theta,~] = cart2sph(region.x,region.y,region.z);
    theta = pi/2 - theta; % transform to 0 <= theta <= pi
    ids = phi > 0;
    Qflux = zeros(size(region.x));
    Qflux(ids) = theta(ids).^2.*(pi - theta(ids)).^2.*phi(ids).^2.*(pi - phi(ids)).^2;
end
```
# Reference


[1] Singh, Suneet, P. K. Jain, and Rizwan-uddin. "Analytical Solution for Three-Dimensional, Unsteady Heat Conduction in a Multilayer Sphere." ASME. J. Heat Transfer. 138(10), 2016, pp. 101301-101301-11. 




*Copyright 2015-2018 The MathWorks, Inc.*


