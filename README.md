# Revit2023

EJES -**********************************
import clr

clr.AddReference('ProtoGeometry')
from Autodesk.DesignScript.Geometry import Solid as DynamoSolido
from Autodesk.DesignScript.Geometry import Line as DynamoLine

clr.AddReference('RevitAPI')
from Autodesk.Revit.DB import *

from Autodesk.Revit.DB import Grid as GridRevit

clr.AddReference('RevitNodes')
import Revit
clr.ImportExtensions(Revit.Elements)
clr.ImportExtensions(Revit.GeometryConversion)

#import Document Manager and TransactionManager
clr.AddReference("RevitServices")
import RevitServices
from RevitServices.Persistence import DocumentManager
from RevitServices.Transactions import TransactionManager

#import math classes
clr.AddReference('DSCoreNodes')
import DSCore
from DSCore import Math

#Variables de Documento
doc = DocumentManager.Instance.CurrentDBDocument
uidoc = DocumentManager.Instance.CurrentUIApplication.ActiveUIDocument

def InterseccionGrillas(grilla1,grilla2):
	linea1 = grilla1.Curve.ToProtoType();
	linea2 = grilla2.Curve.ToProtoType();
	interseccion1 = linea1.Intersect(linea2)
	
	return interseccion1


punto = IN[0]

rejillas1 = FilteredElementCollector(doc)
rejillas1.OfClass(GridRevit).ToElements();

rejillas = list()
for ele in rejillas1:
	rejillas.append(ele)

distanciaComparativa = 999999999999
for i in xrange(len(rejillas)):
	rejillai = rejillas[i]
	for j in xrange(len(rejillas)):
		if not (i==j):
			rejillaj = rejillas[j]
			intersecciones = InterseccionGrillas(rejillai, rejillaj)
			if not(intersecciones.Count==0):
				distancia = punto.DistanceTo(intersecciones[0])
				if distancia<distanciaComparativa:
					distanciaComparativa = distancia
					rejilla1 = rejillai
					rejilla2 = rejillaj
					
				

OUT = rejilla1,rejilla2




/*********************************

solid1 = Revit.Element.Solids(t1);
solid2 = Revit.Element.Solids(t2);
t3 = [solid1, solid2];
solid1 = Solid.ByUnion(t3);
polySurface1 = PolySurface.BySolid(solid1);
surface1 = PolySurface.Surfaces(polySurface1);

num2 = Autodesk.Surface.Area(t1);
surface1 = Revit.Element.Faces(t2);
surface2 = Revit.Element.Faces(t3);
t4 = List.GetItemAtIndex(polySurface1@L2<1>, 0);
directshape1 = DirectShape.ByGeometry(t1, t5, DirectShape.DynamoPreviewMaterial, "DirectShape");


num2 = Autodesk.Surface.Area(t1);
surface1 = Revit.Element.Faces(t2);
surface2 = Revit.Element.Faces(t3);
t4 = List.GetItemAtIndex(polySurface1@L2<1>, 0);
directshape1 = DirectShape.ByGeometry(t1, t5, DirectShape.DynamoPreviewMaterial, "DirectShape");
