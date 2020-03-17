# �u���P�b�g�̂���݉��


���̗�ł́A�L���v�f��́iFEA�j���g�p���āA�׏d����������3�����@�B���i����͂��A�ő傽��݂����߂���@�������܂��B


# �\����̓��f���̍쐬


���`�e�����������ŏ��̃X�e�b�v�́A�\����̓��f�����쐬���邱�Ƃł��B




���̃��f���́A�`��i�W�I���g���j�A�����l�i�\���ޗ��̃v���p�e�B�j�A�����p�����[�^�A���̗́A�׏d�����A�S�������A�L���v�f�̏W���ł���X�[�p�[�G�������g�̃C���^�[�t�F�[�X�A�����ψʂƑ��x�A���b�V�����܂܂��R���e�i�ł��B


```matlab
model = createpde('structural','static-solid');
```
# �`��i�W�I���g���j�̓ǂݍ���


�P���ȃu���P�b�g���f����STL�t�@�C�����A `importGeometry` ���g���ēǂݍ��݂܂��B���̊֐��ɂ��A���f���Ɋ܂܂��t�F�[�X�i�ʁj�A�G�b�W�i�Ӂj�A���@�[�e�b�N�X�i�ߓ_�j���č\������܂��B�������A�������̖ʂ�ӂ͓ǂݍ��ݎ��Ƀ}�[�W�����ꍇ�����邽�߁A���̐��́A���ƂȂ�CAD���f���̂���Ƃ͈قȂ�ꍇ������܂��B


```matlab
importGeometry(model,'BracketWithHole.stl');
```


�ʔԍ���\�����Ȃ���W�I���g����`�悵�܂��B


```matlab
figure
pdegplot(model,'FaceLabels','on')
view(30,30);
title('Bracket with Face Labels')
```

![figure_0.png](StrainedBracketExample_JP_images/figure_0.png)

```matlab
figure
pdegplot(model,'FaceLabels','on')
view(-134,-32)
title('Bracket with Face Labels, Rear View')
```

![figure_1.png](StrainedBracketExample_JP_images/figure_1.png)

# �����l�̎w��


�ޗ��̃����O���ƃ|�A�\������w�肵�܂��B


```matlab
structuralProperties(model,'YoungsModulus',200e9, ...
                           'PoissonsRatio',0.3);
```
# �S��������׏d�����Ƃ��������E�����̐ݒ�


���̖��ł́A�Q�̋��E������ݒ肵�܂��B���ʁi�ʂS�j�����S�S�����A�\�ʂɉ׏d��ݒ肵�܂��B���̑��̓f�t�H���g�ݒ�A�܂�A�������܂���B


```matlab
structuralBC(model,'Face',4,'Constraint','fixed');
```


�\�ʂł���ʂW�ɁA�����z�׏d���[Z�����ɐݒ肵�܂��B


```matlab
structuralBoundaryLoad (model,'Face',8,'SurfaceTraction',[0;0;-1e4]);
```
# ���b�V���̐���


���b�V���𐶐����A�`�悵�܂��B


```matlab
generateMesh(model);
figure
pdeplot3D(model)
title('Mesh with Quadratic Tetrahedral Elements');
```

![figure_2.png](StrainedBracketExample_JP_images/figure_2.png)

# ����


`solve` ���g���ĉ������߂܂��B


```matlab
result = solve(model)
```
```
result = 
  StaticStructuralResults �̃v���p�e�B:

      Displacement: [1x1 FEStruct]
            Strain: [1x1 FEStruct]
            Stress: [1x1 FEStruct]
    VonMisesStress: [5993x1 double]
              Mesh: [1x1 FEMesh]

```
# ����݂̌v�Z


Z�����ł̍ő傽��݂��v�Z���܂��B


```matlab
minUz = min(result.Displacement.uz);
fprintf('Maximal deflection in the z-direction is %g meters.', minUz)
```
```
Maximal deflection in the z-direction is -4.43075e-05 meters.
```
# �ψʗʂ̕`��


����ꂽ���Ɋ܂܂��R���|�l���g��`�悵�܂��BZ�����̂���݂��ł��傫���Ȃ�܂��B����́A�\����͑Ώۂł��镔�i�Ɖ׏d�������Ώ̂ł��邽��X�����̕ψʂ�Z�����̕ψʂ��Ώ̂ɂȂ�AY�����̕ψʂ͒��S���ɑ΂��Ĕ�Ώ̂ɂȂ�܂��B




�����ŁAJET�J���[�}�b�v���g���ĕ`�悵�܂��B���̃J���[�}�b�v�ɂ����Đ͍Œ�l�ɂȂ�A�Ԃ��ő�l�ɂȂ�܂��B�ʂW�ɉ׏d���������Ă邽�߁AZ�����̕ψʂ́A���̖ʂ��ŏ��l�i��Βl�Ƃ��Ă͍ő�j�ɂȂ�܂��B


```matlab
figure
pdeplot3D(model,'ColorMapData',result.Displacement.ux)
title('x-displacement')
colormap('jet')
```

![figure_3.png](StrainedBracketExample_JP_images/figure_3.png)

```matlab

figure
pdeplot3D(model,'ColorMapData',result.Displacement.uy)
title('y-displacement')
colormap('jet')
```

![figure_4.png](StrainedBracketExample_JP_images/figure_4.png)

```matlab

figure
pdeplot3D(model,'ColorMapData',result.Displacement.uz)
title('z-displacement')
colormap('jet')
```

![figure_5.png](StrainedBracketExample_JP_images/figure_5.png)

# �t�H���E�~�[�[�X���͂̕`��


�t�H���E�~�[�[�X���͂�ߓ_�ʒu�ɕ`�悵�܂��BJET�J���[�}�b�v���g�p���܂��B


```matlab
figure
pdeplot3D(model,'ColorMapData',result.VonMisesStress)
title('von Mises stress')
colormap('jet')
```

![figure_6.png](StrainedBracketExample_JP_images/figure_6.png)



*Copyright 2014-2020 The MathWorks, Inc.*




*Japanese edition is developped by Dr. Hiroyuki HISHIDA *


