import { useState } from "react";
import { Button } from "@/components/ui/button";
import { Card } from "@/components/ui/card";
import { Minus, Plus, Trash2, Printer, ShoppingCart } from "lucide-react";
import { toast } from "sonner";

// Import dish images
import idliImg from "@/assets/dishes/idli.jpg";
import parottaImg from "@/assets/dishes/parotta.jpg";
import tomatoRiceImg from "@/assets/dishes/tomato-rice.jpg";
import chickenBiryaniImg from "@/assets/dishes/chicken-biryani.jpg";
import chilliChickenImg from "@/assets/dishes/chilli-chicken.jpg";
import omeletteImg from "@/assets/dishes/omelette.jpg";
import dosaImg from "@/assets/dishes/dosa.jpg";
import puriImg from "@/assets/dishes/puri.jpg";
import pongalImg from "@/assets/dishes/pongal.jpg";
import whiteRiceImg from "@/assets/dishes/white-rice.jpg";
import chickenGravyImg from "@/assets/dishes/chicken-gravy.jpg";
import curdRiceImg from "@/assets/dishes/curd-rice.jpg";
import lemonRiceImg from "@/assets/dishes/lemon-rice.jpg";
import vadaiImg from "@/assets/dishes/vadai.jpg";
import bondaImg from "@/assets/dishes/bonda.jpg";
import eggPuffImg from "@/assets/dishes/egg-puff.jpg";
import chickenNoodlesImg from "@/assets/dishes/chicken-noodles.jpg";
import eggRiceImg from "@/assets/dishes/egg-rice.jpg";

interface Dish {
  id: string;
  name: string;
  price: number;
  image: string;
}

interface BillItem extends Dish {
  quantity: number;
}

const dishes: Dish[] = [
  { id: "1", name: "idli", price: 5, image: idliImg },
  { id: "2", name: "parotta", price: 15, image: parottaImg },
  { id: "3", name: "tomato rice", price: 30, image: tomatoRiceImg },
  { id: "4", name: "chicken biriyani", price: 100, image: chickenBiryaniImg },
  { id: "5", name: "chilli chicken", price: 70, image: chilliChickenImg },
  { id: "6", name: "Amlet", price: 10, image: omeletteImg },
  { id: "7", name: "dosa", price: 10, image: dosaImg },
  { id: "8", name: "puri", price: 12, image: puriImg },
  { id: "9", name: "pongal", price: 30, image: pongalImg },
  { id: "10", name: "White rice", price: 80, image: whiteRiceImg },
  { id: "11", name: "chicken geravi", price: 80, image: chickenGravyImg },
  { id: "12", name: "curd rice", price: 25, image: curdRiceImg },
  { id: "13", name: "lemon rice", price: 25, image: lemonRiceImg },
  { id: "14", name: "vadai", price: 5, image: vadaiImg },
  { id: "15", name: "Bonda", price: 5, image: bondaImg },
  { id: "16", name: "Egg puffs", price: 15, image: eggPuffImg },
  { id: "17", name: "chicken noodles", price: 100, image: chickenNoodlesImg },
  { id: "18", name: "egg rice", price: 80, image: eggRiceImg },
];

const Index = () => {
  const [billItems, setBillItems] = useState<BillItem[]>([]);

  const addToBill = (dish: Dish) => {
    setBillItems((prev) => {
      const existing = prev.find((item) => item.id === dish.id);
      if (existing) {
        return prev.map((item) =>
          item.id === dish.id ? { ...item, quantity: item.quantity + 1 } : item
        );
      }
      return [...prev, { ...dish, quantity: 1 }];
    });
    toast.success(`Added ${dish.name} to bill`);
  };

  const updateQuantity = (id: string, delta: number) => {
    setBillItems((prev) =>
      prev
        .map((item) =>
          item.id === id ? { ...item, quantity: Math.max(0, item.quantity + delta) } : item
        )
        .filter((item) => item.quantity > 0)
    );
  };

  const removeItem = (id: string) => {
    setBillItems((prev) => prev.filter((item) => item.id !== id));
    toast.info("Item removed from bill");
  };

  const clearBill = () => {
    setBillItems([]);
    toast.info("Bill cleared");
  };

  const printBill = () => {
    window.print();
    toast.success("Opening print dialog...");
  };

  const total = billItems.reduce((sum, item) => sum + item.price * item.quantity, 0);

  return (
    <div className="min-h-screen bg-background">
      <header className="bg-card border-b border-border sticky top-0 z-10 print:hidden">
        <div className="container mx-auto px-4 py-4">
          <h1 className="text-2xl md:text-3xl font-bold text-primary">Nanbrgal Unavagam</h1>
          <p className="text-sm text-muted-foreground">Select dishes to add to your bill</p>
        </div>
      </header>

      <main className="container mx-auto px-4 py-6">
        <div className="grid grid-cols-1 lg:grid-cols-3 gap-6">
          {/* Menu Section */}
          <section className="lg:col-span-2 print:hidden">
            <h2 className="text-xl font-semibold mb-4 text-foreground">Menu</h2>
            <div className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-4">
              {dishes.map((dish) => (
                <Card
                  key={dish.id}
                  className="overflow-hidden cursor-pointer transition-all hover:shadow-lg"
                  onClick={() => addToBill(dish)}
                  style={{ boxShadow: "var(--shadow-card)" }}
                  onMouseEnter={(e) => {
                    e.currentTarget.style.boxShadow = "var(--shadow-card-hover)";
                  }}
                  onMouseLeave={(e) => {
                    e.currentTarget.style.boxShadow = "var(--shadow-card)";
                  }}
                >
                  <div className="aspect-[4/3] overflow-hidden bg-muted">
                    <img
                      src={dish.image}
                      alt={dish.name}
                      loading="lazy"
                      className="w-full h-full object-cover transition-transform hover:scale-105"
                    />
                  </div>
                  <div className="p-4">
                    <h3 className="font-semibold text-foreground capitalize mb-1">{dish.name}</h3>
                    <p className="text-lg font-bold text-primary">₹{dish.price}</p>
                  </div>
                </Card>
              ))}
            </div>
          </section>

          {/* Bill Section */}
          <section className="lg:sticky lg:top-24 h-fit">
            <Card className="p-6">
              <div className="flex items-center gap-2 mb-4">
                <ShoppingCart className="w-5 h-5 text-primary" />
                <h2 className="text-xl font-semibold text-foreground">Bill</h2>
              </div>

              {billItems.length === 0 ? (
                <div className="text-center py-8 text-muted-foreground">
                  <p>No items added yet</p>
                  <p className="text-sm mt-2">Click on dishes to add them to your bill</p>
                </div>
              ) : (
                <>
                  <div className="space-y-3 mb-6 max-h-[400px] overflow-y-auto">
                    {billItems.map((item) => (
                      <div key={item.id} className="flex items-center justify-between gap-3 p-3 bg-muted rounded-lg">
                        <div className="flex-1 min-w-0">
                          <p className="font-medium text-foreground capitalize truncate">{item.name}</p>
                          <p className="text-sm text-muted-foreground">₹{item.price} each</p>
                        </div>
                        <div className="flex items-center gap-2">
                          <Button
                            size="icon"
                            variant="outline"
                            className="h-8 w-8"
                            onClick={() => updateQuantity(item.id, -1)}
                          >
                            <Minus className="w-3 h-3" />
                          </Button>
                          <span className="w-8 text-center font-semibold">{item.quantity}</span>
                          <Button
                            size="icon"
                            variant="outline"
                            className="h-8 w-8"
                            onClick={() => updateQuantity(item.id, 1)}
                          >
                            <Plus className="w-3 h-3" />
                          </Button>
                          <Button
                            size="icon"
                            variant="ghost"
                            className="h-8 w-8 text-destructive hover:text-destructive"
                            onClick={() => removeItem(item.id)}
                          >
                            <Trash2 className="w-4 h-4" />
                          </Button>
                        </div>
                        <div className="text-right min-w-[60px]">
                          <p className="font-semibold text-foreground">₹{item.price * item.quantity}</p>
                        </div>
                      </div>
                    ))}
                  </div>

                  <div className="border-t border-border pt-4 mb-4">
                    <div className="flex justify-between items-center mb-2">
                      <span className="text-muted-foreground">Subtotal</span>
                      <span className="font-semibold text-foreground">₹{total}</span>
                    </div>
                    <div className="flex justify-between items-center text-lg font-bold">
                      <span className="text-foreground">Total</span>
                      <span className="text-primary">₹{total}</span>
                    </div>
                  </div>

                  <div className="flex gap-2 print:hidden">
                    <Button onClick={printBill} className="flex-1" size="lg">
                      <Printer className="w-4 h-4 mr-2" />
                      Print
                    </Button>
                    <Button onClick={clearBill} variant="outline" size="lg">
                      Clear
                    </Button>
                  </div>
                </>
              )}
            </Card>
          </section>
        </div>
      </main>

      {/* Print Styles */}
      <style>{`
        @media print {
          body {
            background: white;
          }
          .print\\:hidden {
            display: none !important;
          }
          main {
            padding: 20px;
          }
          .lg\\:col-span-2 {
            display: none;
          }
          .lg\\:sticky {
            position: static;
          }
        }
      `}</style>
    </div>
  );
};

export default Index;
