 class XmlMerger{
 private static bool IsSameTag(XElement a, XElement b)
        {
            if (a.Name != b.Name)
                return false;
            IEnumerable<XAttribute> t1 = a.Attributes();
            IEnumerable<XAttribute> t2 = b.Attributes();

            if (t1.Count() != t2.Count())
                return false;
            else
            {
                List<XAttribute> tag1 = new List<XAttribute>(); List<XAttribute> tag2 = new List<XAttribute>();
                foreach (var p in t1)
                {
                    tag1.Add(p);
                }
                foreach (var q in t2)
                {
                    tag2.Add(q);
                }

                for (int i = 0; i < tag1.Count(); i++)
                {
                    if ((tag1[i].Name != tag2[i].Name) || (tag1[i].Value != tag2[i].Value))
                    {
                        return false;
                    }
                }
            }

            return true;
        }

        private static bool hasSameTag(XElement ele , List<XElement> lis)
        {
            foreach (var v in lis)
            {
                if (IsSameTag(ele, v))
                    return true;
            }

            return false;
        }
        private static XElement Merge( XElement a, XElement b)
        {
            List<XElement> child = new List<XElement>();
           //Both Non-Leaf nodes
            if (a.Elements().Any() && b.Elements().Any())
            {
                if (IsSameTag(a, b))
                {
                    List<XElement> la = a.Elements().ToList();
                    List<XElement> lb = b.Elements().ToList();
                    List<XName> commNames = new List<XName>();
                    List<XElement> commElements =new List<XElement>();
                    XElement mergedElement = null;
                    foreach (var p in la)
                    {
                        foreach (var q in lb)
                        {
                            if (IsSameTag(p, q))
                            {

                                mergedElement = Merge( p, q);
                                
                                child.Add(mergedElement);
                                commElements.Add(mergedElement);
                                commNames.Add(p.Name);

                            }

                        }
                    }
                    foreach (var z in la)
                    {
                        if (commNames.Contains(z.Name) && hasSameTag(z,commElements))
                        {
                            
                        }
                        else
                        {
                            child.Add(z);
                        }
                    }
                    foreach (var c in lb)
                    {
                        if (commNames.Contains(c.Name) && hasSameTag(c,commElements))
                        {
          
                        }
                        else
                        {
                            child.Add(c);
                        }
                    }
                    XElement result = new XElement(a.Name,a.Attributes(), child);
                    return result;
                }
                else
                {
                    return null;
                }

            }
            //both leaf
            else if (!a.Elements().Any() && !b.Elements().Any())
            {

                return a;
            }
            //b non-leaf,a leaf
            else if (!a.Elements().Any() && b.Elements().Any())
            {
                return b;
            }
            //a non-leaf,b leaf
            else
            {
                return a;
            }
        }
        public static void MergeFiles(string filepath1, string filepath2,string mergedFilePath)
        {
            var xml1 = XDocument.Load(filepath1);
            var xml2 = XDocument.Load(filepath2);
            var xml3 = XDocument.Load(mergedFilePath);
            xml3.Root.Descendants().Remove();
            xml3.Save(mergedFilePath);

            var z1 = xml1.Root;
            var z2 = xml2.Root;
            XElement rootElement = Merge(z1, z2);
            XDocument xdoc = new XDocument(rootElement);
            xml3 = xdoc;
            xml3.Save(mergedFilePath);
            AppConfig.Change(mergedFilePath);
        }
        static void Main()
        {
            MergeFiles("C:/Users/320050487/source/repos/MergingXMLsProd/MergingXMLsProd/a.xml",
                "C:/Users/320050487/source/repos/MergingXMLsProd/MergingXMLsProd/b.xml",
                "C:/Users/320050487/source/repos/MergingXMLsProd/MergingXMLsProd/merged.xml");
            //var setting = ConfigurationManager.AppSettings["ModulesInfoFolder"];
            //Console.WriteLine("value of Key ModulesInfoFolder =" + setting);
            Console.ReadLine();
        }
    }
