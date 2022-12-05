global proc GroupConstraintsToolUI()
{
    if(`window -exists "GCTool"` == true)
    deleteUI "GCTool" ;
    window -title "Group Constraints Tool ver.1.0" "GCTool" ;
    columnLayout;
    button -label "1. Set parents" -width 295 -c "setparents" ;
    button -label "2-1. One to Multi" -width 295 -c "one2multi" ;
    button -label "2-2. Multi to Multi" -width 295 -c "multi2multi" ;
    button -label "Process Reset" -width 295 -c "reset" ;
    showWindow "GCTool" ;
}
GroupConstraintsToolUI() ;

global proc setparents()
{
    string $parents[] = `ls -sl` ;
    for ($i=0; $i<size($parents); $i++)
        {
            CreateLocator ;
            string $Loc[] = `ls -sl` ;
            rename $Loc[0] ("ConstraintLoc" + $i) ;
            string $Loc[] = `ls -sl` ;
            select -tgl $parents[$i] ;
            parent ;
        }
}

global proc one2multi()
{
    string $children[] = `ls -sl` ;
    for ($i=0; $i<size($children); $i++)
        {
            select ConstraintLoc0 ;
            pickWalk -d up;
            string $parents[] = `ls -sl` ;
            select -r $parents[0] ;
            select -tgl $children[$i] ;
            parentConstraint -mo -weight 1;
        }
     select ("ConstraintLoc*");
     doDelete ;
}

global proc multi2multi()
{
    string $children[] = `ls -sl` ;
    for ($i=0; $i<size($children); $i++)
        {
            select ("ConstraintLoc*");
            pickWalk -d up;
            string $parents[] = `ls -sl` ;
            select -r $parents[$i] ;
            select -tgl $children[$i] ;
            parentConstraint -mo -weight 1;
        }
     select ("ConstraintLoc*");
     doDelete ;
}

global proc reset()
{
     select ("ConstraintLoc*");
     doDelete ;
}
